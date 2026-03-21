# SDE2 System Design Interview — The Only Guide You Need

> **Target:** SDE2 (Amazon L5) / Google L4 / Meta E4, Microsoft, Apple, Uber, Netflix, Anthropic, OpenAI
> **Timeline:** 4–8 weeks of focused preparation
> **Last Updated:** March 2026

---

### ⚡ SDE2 Focus Guide — Read This First

> This guide is calibrated for **SDE2 only** (Amazon L5 / Google L4 / Meta E4). Content that exceeds SDE2 expectations is marked with 🔶 **STRETCH** — skip it on first pass, revisit only if you have extra time.
>
> **What SDE2 interviewers actually evaluate** (per systemdesignhandbook.com, Blind, interviewing.io):
> - Can you take a feature/component and design a scalable solution? ✅
> - Can you articulate trade-offs clearly? ✅
> - Can you do back-of-envelope math? ✅
> - Can you identify failure modes? ✅
> - Can you paint the big picture correctly? ✅
>
> **What SDE2 interviewers do NOT penalize you for missing:**
> - Multi-region active-active architecture ❌
> - Cross-org system design ❌
> - Migration strategies from legacy systems ❌
> - Deep consensus algorithm internals (Raft/Paxos) ❌
> - Cost optimization across regions ❌
> - 3-5 year forward architecture planning ❌
>
> **The bar in one sentence:** "Design a reliable, well-architected component within a single service or team boundary, explain your trade-offs, and handle scale within a known range."

### 📋 Prerequisites Checklist (Do You Have the Foundation?)

> Before diving in, verify you're comfortable with these basics. If any row is ❌, fill the gap first — links provided.

| Area | You Should Know | Quick Check | If ❌, Start Here |
|------|----------------|-------------|-------------------|
| **Networking** | HTTP methods, TCP vs UDP, what DNS does | "What happens when you type google.com?" | [ByteByteGo — What happens when you type a URL](https://www.youtube.com/watch?v=AlkDbnbv7dk) |
| **OS Basics** | Processes vs threads, memory vs disk, what a port is | "Why is reading from RAM faster than disk?" | [Neso Academy — OS Basics](https://www.youtube.com/playlist?list=PLBlnK6fEyqRiVhbXDGLXDk_OQAdc0cPiS) (first 10 videos) |
| **Data Structures** | HashMap, LinkedList, Tree, Queue, Heap — when to use each | "How does a HashMap achieve O(1) lookup?" | [NeetCode — DS Basics](https://neetcode.io/courses/dsa-for-beginners) |
| **Basic SQL** | SELECT, JOIN, INDEX, PRIMARY KEY, FOREIGN KEY | "Write a query to get the top 5 orders by amount for user X" | [SQLBolt — Interactive SQL](https://sqlbolt.com/) |
| **OOP** | Classes, interfaces, inheritance, polymorphism, encapsulation | "What's the difference between an interface and an abstract class?" | [Refactoring.Guru — OOP Basics](https://refactoring.guru/design-patterns/what-is-a-pattern) |

### 🗺️ Visual Learning Path (Your GPS Through This Guide)

> Don't read linearly. Follow this path based on your experience level.

```
SDE1 → SDE2 Path (6 weeks):
═══════════════════════════
Week 1-2: CONCEPTS                    Week 3-4: PROBLEMS                  Week 5-6: PRACTICE
┌─────────────────────┐              ┌─────────────────────┐             ┌──────────────────┐
│ Scaling (#1-6)      │──────────▶   │ Easy HLD:           │─────────▶  │ Mock Interview #1│
│ Data Storage (#7-14)│              │  URL Shortener       │             │ Mock Interview #2│
│ Caching (#19-25)    │              │  Rate Limiter        │             │ Mock Interview #3│
│ Communication(#26-34)              │  Notifications       │             │ Review weak areas│
│ Dist Systems(#35-49)│              │ Medium HLD:          │             │ Company-specific │
│ Reliability (#50-56)│              │  Chat, Feed, YouTube │             │  prep            │
│ Security (#57-59)   │              │ LLD: Parking Lot,    │             └──────────────────┘
│ API Design (#60-64) │              │  LRU Cache, Elevator │
└─────────────────────┘              └─────────────────────┘

Working SDE → SDE2 Path (2 weeks):
══════════════════════════════════
Week 1: HLD                           Week 2: LLD + MOCKS
┌─────────────────────┐              ┌─────────────────────┐
│ Skip concepts you   │──────────▶   │ SOLID + 5 patterns  │
│  use daily at work  │              │ Parking Lot, LRU,   │
│ Deep-read: CAP,     │              │  Elevator, Vending  │
│  consistent hashing,│              │ 3 mock interviews   │
│  fan-out, Sagas     │              │ Company-specific    │
│ 7 HLD problems      │              │  review             │
└─────────────────────┘              └─────────────────────┘
```

### 🧠 How to Think, Not What to Memorize

> This guide shows you what good answers look like. But interviewers don't reward memorized answers — they reward **live reasoning**. Here's how to generate good answers for problems you've never seen.

**The Decision Framework (use this for EVERY technology choice):**
1. **What's the access pattern?** — Read-heavy? Write-heavy? Both? → Drives DB, caching, and indexing choices
2. **What's the consistency requirement?** — Can users see stale data? → Drives CP vs AP, sync vs async
3. **What's the scale?** — 1K QPS or 100K QPS? → Drives whether you need sharding, caching, queues
4. **What's the failure impact?** — Lost message = annoyed user or lost money? → Drives durability and redundancy choices
5. **What's the operational complexity?** — Can your team run this? → Drives managed service vs self-hosted

*Example: "Should I use Kafka or SQS?" → Ask: Do I need message replay? (Kafka). Do I need simple fire-and-forget? (SQS). Is my team already running Kafka? (use it). Is this a new service with no infra? (SQS — less ops burden).*

**When you're stuck in an interview:**
- **Don't know a technology?** Describe what you need: *"I need an in-memory store with TTL and LRU eviction"* — the interviewer will nod. Never guess a name.
- **Don't know how to proceed?** Zoom out: *"Let me revisit the requirements — which constraint should I prioritize?"*
- **Interviewer challenges your choice?** Don't defend — explore: *"Good point. If we went with that approach, the trade-off would be [X]. I chose my approach because [Y] matters more for this use case."*
- **Drawing a blank?** Buy time with structure: *"Let me think about the read path and write path separately."*

**Anti-memorization rule:** Every "interview talking point" in this guide is an *example*, not a script. If you catch yourself reciting one verbatim, stop and ask: "Why is this the right choice for THIS specific problem?" If you can't answer that, you don't understand it yet.

### 🚀 Already Working as an SDE? Start Here

> You already know a chunk of this guide from your day job. Don't read it cover-to-cover. Here's your fast path:

**Skip these concepts if you use them daily at work:**
- #1-3 (Scaling, Load Balancing, Auto-Scaling)
- #5 (API Gateway)
- #7 (SQL vs NoSQL) — if you already pick between relational and NoSQL confidently
- #9 (Replication) — skim the terminology even if you know the concept
- #17 (Blob Storage) — S3/GCS/Azure Blob
- #22 (CDN)
- #50-52 (SPOF, Failover, Health Checks)

**Read these (common blind spots for working SDEs):**
- #6 (Back-of-Envelope Math) — you've never done this in an interview format
- #12 (Isolation Levels) — if you mostly use NoSQL, this is a gap
- #13-14 (CAP Theorem, Consistent Hashing) — you know the result, not the theory
- #18 (Data Modeling) — can you model for BOTH SQL and NoSQL under time pressure?
- #30 (WebSocket Management at Scale) — deep dive most SDEs never need at work
- #66 (Thread Safety) — rusty if your services are serverless or single-threaded

**Your actual study plan:** Jump to [Part 7: The 2-Week Sprint](#part-7-preparation-schedule) first. Come back to this guide as a reference, not a textbook.

### ⏱️ Interview Pacing Guide (The 45-Minute Clock)

> Most candidates fail on time management, not knowledge. Here's how to pace yourself:

```
[0:00 - 0:05]  REQUIREMENTS (5 min)
  "Before I design, let me clarify..."
  • 3-4 functional requirements (what the system does)
  • 2-3 non-functional requirements (scale, latency, availability)
  • Back-of-envelope: QPS, storage, bandwidth (2 min max — narrate while writing)
  • Confirm scope: "Does this cover what you'd like, or should I adjust?"

[0:05 - 0:15]  HIGH-LEVEL DESIGN (10 min)
  Draw the big picture. Don't overthink component choices yet.
  • Client → LB → API Servers → DB/Cache (the skeleton)
  • Define 2-3 core APIs (method + params + response — NOT full request bodies)
  • Define data model (entity names + key fields + relationships — NOT full CREATE TABLE)
  • Show read path and write path separately

  ⚠️ CHECKPOINT: "I have the high-level design. Which area would you like me to dive into?"
  (This is the most important sentence in the interview. It shows maturity and lets the
  interviewer steer toward what THEY want to evaluate.)

[0:15 - 0:35]  DEEP DIVE (20 min)
  Go deep on 2-3 components. This is where you win or lose.
  • Pick the hardest/most interesting component
  • Discuss alternatives: "I considered X and Y, chose X because..."
  • Address failure modes: "If this component goes down..."
  • Handle scale: "At 100K QPS, we'd need to shard by..."

  ⚠️ "GOOD ENOUGH" RULE: If you've spent 7+ minutes on one component, MOVE ON.
  Say: "I could go deeper here, but let me cover [next component] to give you
  a complete picture. Happy to come back to this."

[0:35 - 0:40]  WRAP UP (5 min)
  • "The main bottleneck is [X], I'd address it with [Y]"
  • "I'd monitor p99 latency, error rate, and [system-specific metric]"
  • "Future extensions: [1-2 things you'd add next]"
```

**How detailed should things be?**

| Element | Level of Detail | Example |
|---------|----------------|---------|
| **APIs** | Method + key params + response type | `POST /urls {long_url} → {short_url}` — NOT full JSON schema |
| **Data Model** | Entity + key columns + relationships | `users(id, name, email)`, `messages(id, sender_id, chat_id, text, timestamp)` — NOT full DDL |
| **Back-of-envelope** | Narrate while writing, 2 min max | "100M DAU × 10 msgs/day = 1B msgs/day ÷ 86400 ≈ 12K QPS" — on the whiteboard |
| **Architecture diagram** | Boxes + arrows + labels | Don't draw perfect diagrams. Messy but complete > pretty but incomplete |

---

## Table of Contents

- [Part 0: HLD vs LLD — What's the Difference?](#part-0-hld-vs-lld)
- [Part 1: Foundational Concepts (76 Topics)](#part-1-foundational-concepts)
  - [1A. Scaling & Traffic Management (#1-6)](#1a-scaling--traffic-management)
  - [1B. Data Storage & Data Modeling (#7-18)](#1b-data-storage--data-modeling)
  - [1C. Caching (#19-25)](#1c-caching) — includes Hot Key Problem, Cache Warming, Cache Invalidation
  - [1D. Communication & Networking (#26-34)](#1d-communication--networking) — includes DNS Fundamentals
  - [1E. Distributed Systems Concepts (#35-49)](#1e-distributed-systems-concepts) — includes Saga Pattern, Outbox, CDC
  - [1F. Reliability & Observability (#50-56)](#1f-reliability--observability) — includes Deployment Patterns, Load Shedding
  - [1G. Security Basics (#57-59)](#1g-security-basics) — includes OAuth 2.0, JWT, Token Refresh
  - [1H. API Design Fundamentals (#60-64)](#1h-api-design-fundamentals)
  - [1I. Cost Awareness (#65)](#1i-cost-awareness)
  - [1J. Concurrency & Thread Safety (#66-69)](#1j-concurrency--thread-safety)
  - [1K. Commonly Missed Concepts (#70-76)](#1k-commonly-missed-concepts) — includes Search Internals
- [Part 2: HLD Problems — "Design X" (37 Problems)](#part-2-hld-problems)
  - [Fully Worked Example: URL Shortener](#fully-worked-example-url-shortener) — includes Bad vs Good Answer
  - [Tier 1: Must-Do (10 problems)](#tier-1-must-do) — with 🟢🟡🔴 difficulty ratings + common mistakes
  - [Tier 2: High Frequency (10 problems)](#tier-2-high-frequency)
  - [Tier 3: Moderate Frequency (12 problems)](#tier-3-moderate-frequency) — 🔶 STRETCH
  - [Tier 4: Emerging / AI-Era (5 problems)](#tier-4-emerging) — 🔶 STRETCH
- [Part 3: LLD Problems — Object-Oriented Design (25 Problems)](#part-3-lld-problems)
  - [5-Minute LLD Derivation Method](#the-5-minute-lld-derivation-method)
  - [UML Basics for Interviews](#uml-basics-for-interviews)
  - [Fully Worked: Parking Lot](#fully-worked-parking-lot)
  - [Fully Worked: LRU Cache](#fully-worked-lru-cache)
  - [Fully Worked: Elevator System](#fully-worked-elevator-system)
  - [Tier 1 (8 problems)](#tier-1-most-frequently-asked) | [Tier 2 (9 problems)](#tier-2-high-frequency-1) | [Tier 3 (8 problems)](#tier-3-occasionally-asked)
- [Part 4: Design Patterns for LLD (14 patterns + SOLID)](#part-4-design-patterns)
- [Part 5: The Interview Framework + Trade-off Mastery](#part-5-interview-framework) — includes Mock Interview Rubric
- [Part 6: Company-Specific Notes](#part-6-company-specific-notes) — FAANG + Remote/Startup companies
- [Part 7: Preparation Schedule](#part-7-preparation-schedule) — 2-week, 4-week, and 6-week (SDE1→SDE2) plans
- [Part 8: Master Resource List + Glossary](#part-8-master-resource-list)

---

## Master Resource Links (Bookmark These)

| Resource | Type | Link |
|----------|------|------|
| ByteByteGo YouTube | Video | https://www.youtube.com/@ByteByteGo |
| Gaurav Sen YouTube | Video | https://www.youtube.com/@gaborsen |
| System Design Interview (Alex Xu) Book | Book | https://www.amazon.com/System-Design-Interview-insiders-Second/dp/B08CMF2CQF |
| System Design Interview Vol 2 (Alex Xu) | Book | https://www.amazon.com/System-Design-Interview-Insiders-Guide/dp/1736049119 |
| Designing Data-Intensive Applications (Kleppmann) | Book | https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321 |
| DesignGurus.io | Course | https://www.designgurus.io |
| HelloInterview | Practice | https://www.hellointerview.com/learn/system-design |
| SystemDesignSchool | Practice | https://systemdesignschool.io |
| AlgoMaster LLD | Practice | https://algomaster.io/learn/lld |
| Refactoring.Guru (Patterns) | Reference | https://refactoring.guru/design-patterns |
| Codemia.io | Practice | https://www.codemia.io |
| sudoCODE YouTube | Video | https://www.youtube.com/@sudocode |
| codeKarle YouTube | Video | https://www.youtube.com/@codeKarle |
| Tech Dummies YouTube | Video | https://www.youtube.com/@TechDummiesNarendraL |

---

## Part 0: HLD vs LLD

### They Are NOT the Same

| Aspect | HLD (High-Level Design) | LLD (Low-Level Design) |
|--------|------------------------|----------------------|
| **What** | Architecture of a distributed system | Internal code structure — classes, interfaces, schemas |
| **Scope** | Millions of users, global scale | Single service or module implementation |
| **You Draw** | Load balancers, DBs, caches, queues, CDNs, services | Class diagrams, sequence diagrams, API signatures |
| **Trade-offs** | CAP theorem, consistency vs availability, cost vs perf | Design patterns, SOLID principles, extensibility |
| **Example Q** | "Design WhatsApp" | "Design a Parking Lot system" |
| **Time Split** | ~40 min interview | ~40 min interview |

### What SDE2 Must Cover

At SDE2 level, you're expected to handle **both** HLD and LLD.

**How companies structure it (2025-2026 data):**
- **Amazon:** Typically 1 dedicated system design round (HLD with hiring manager) + 1 "Logical and Maintainable" coding round that often includes OOD/LLD elements. Some teams do 2 SD rounds. Always confirm structure with your recruiter.
- **Google L4:** System design round is **not guaranteed** — it depends on the team and hiring committee. When present, expectations are lower than L5. Focus on clean, maintainable architecture rather than planet-scale distributed systems.
- **Meta E4:** 1 system design round. You may choose between system design and product architecture. Focus on scale (billions of users).
- **Microsoft/Apple/Uber:** 1-2 system design rounds, varying mix of HLD and LLD.

**HLD expectations at SDE2:**
- Drive the conversation (don't wait for hints)
- Define functional + non-functional requirements unprompted
- Do back-of-envelope math
- Discuss trade-offs with depth (not just "use Redis")
- Handle scale (millions of users)
- Consider failure modes proactively

**LLD expectations at SDE2:**
- Model real-world entities as classes with clear relationships
- Apply appropriate design patterns (not force-fit them)
- Define clean APIs and method signatures
- Handle edge cases and validations
- Write extensible, maintainable code structure

---

## Part 1: Foundational Concepts (76 Topics)

> You cannot design anything without these. For each concept: understand what it is, when to use it, trade-offs, and be able to explain it in 2 minutes during an interview.
>
> **How to use this section:** Each concept ends with a ✅ **You're done when** marker. That's your SDE2 bar. If you can do what it says, move on. Don't go deeper.

---

### 1A. Scaling & Traffic Management

#### 1. Horizontal vs Vertical Scaling

**What:** Vertical = bigger machine (more CPU/RAM). Horizontal = more machines.

**When to use what:**
- Vertical: Simple, works for small-medium scale. Has a hard ceiling.
- Horizontal: Required for large-scale systems. Adds complexity (data consistency, load balancing).

**Interview talking point:** "Vertical scaling has a hardware ceiling and creates a single point of failure. For any system serving millions of users, horizontal scaling is the default — we add stateless servers behind a load balancer."

**Trade-off:** Vertical is simpler but limited. Horizontal is scalable but requires stateless design, distributed data, and load balancing.

✅ **You're done when:** You can say "Vertical has a ceiling and is a SPOF. For any system at scale, horizontal scaling with stateless servers behind a load balancer." That's the full answer. Don't memorize hardware specs.

📺 [Gaurav Sen — Horizontal vs Vertical Scaling](https://www.youtube.com/watch?v=xpDnVSmNFX0)
📖 [ByteByteGo — Scaling](https://blog.bytebytego.com/p/ep56-system-design-101)

---

#### 2. Load Balancing

**What:** Distributes incoming traffic across multiple servers to prevent any single server from being overwhelmed.

**Algorithms:**
- **Round Robin** — Requests go to servers in rotation. Simple, works when servers are identical.
- **Least Connections** — Routes to server with fewest active connections. Good for long-lived connections.
- **Weighted Round Robin** — Servers with more capacity get more traffic.
- **IP Hash** — Same client always goes to same server (session affinity).
- **Consistent Hashing** — Minimizes redistribution when servers are added/removed.

**Layers:**
- L4 (Transport) — Routes based on IP/port. Faster, less flexible.
- L7 (Application) — Routes based on HTTP headers, URL, cookies. More flexible, slightly slower.

**Interview talking point:** "I'd place an L7 load balancer in front of our API servers. For most cases, round robin with health checks is sufficient. If we need session stickiness, we can use IP hash or move session state to Redis."

✅ **You're done when:** You know L4 vs L7 difference (L4 = IP/port, L7 = HTTP headers/URL), can name 2-3 algorithms (round robin, least connections, consistent hashing), and know when to pick which. You do NOT need to know how LBs are implemented internally.

📺 [ByteByteGo — Load Balancing](https://www.youtube.com/watch?v=sCR3SAVdyCc)
📖 [IGotAnOffer — Load Balancing Deep Dive](https://igotanoffer.com/blogs/tech/load-balancing-system-design-interview)

---

#### 3. Auto-Scaling

**What:** Automatically adjusts the number of running instances based on traffic/load metrics (CPU, memory, request count).

**Key concepts:**
- Scale-out trigger: e.g., CPU > 70% for 5 min → add 2 instances
- Scale-in trigger: e.g., CPU < 30% for 10 min → remove 1 instance
- Cooldown period: Prevents thrashing (rapid scale up/down)

**Interview talking point:** "Our API servers are stateless, so we can auto-scale horizontally. We'd set a target tracking policy on average CPU utilization at 60%, with a cooldown of 300 seconds."

✅ **You're done when:** You can say "stateless servers + auto-scale on CPU/QPS with a cooldown period." One sentence is enough. Never a standalone interview question.

---

#### 4. Rate Limiting

**What:** Controls how many requests a client can make in a given time window. Protects services from abuse and overload.

**Algorithms:**
| Algorithm | How It Works | Pros | Cons |
|-----------|-------------|------|------|
| **Token Bucket** | Tokens added at fixed rate. Each request consumes a token. | Allows bursts, smooth | Needs tuning |
| **Leaky Bucket** | Requests queued, processed at fixed rate | Smooth output | No burst handling |
| **Fixed Window** | Count requests per fixed time window (e.g., per minute) | Simple | Boundary burst problem |
| **Sliding Window Log** | Track timestamp of each request | Accurate | Memory heavy |
| **Sliding Window Counter** | Hybrid of fixed window + weighted previous window | Good balance | Approximate |

**Where to place:** API Gateway level (global), or per-service (local).

**Interview talking point:** "I'd implement rate limiting at the API Gateway using a token bucket algorithm backed by Redis for distributed counting. Each API key gets 100 requests/minute. We return HTTP 429 when exceeded."

✅ **You're done when:** You can explain token bucket in 30 seconds ("tokens added at fixed rate, each request consumes one, rejected when empty"), know where to place it (API Gateway), and know it uses Redis for distributed counting. You do NOT need to implement sliding window log from scratch.

🧪 **Self-test:**
- *Your API gets 10K req/sec but limit is 1K. Which algorithm handles bursts best?* → Token bucket (allows short bursts up to bucket size)
- *Why Redis and not local memory for rate limiting?* → Distributed — multiple API servers need shared counters
- *Fixed window has a "boundary burst" problem. What is it?* → 1K requests at 0:59 + 1K at 1:00 = 2K in 2 seconds, bypassing the limit

📺 [ByteByteGo — Rate Limiter](https://www.youtube.com/watch?v=FU4WlwfS3G0)
📖 [HelloInterview — Rate Limiter Design](https://www.hellointerview.com/learn/system-design/problem-breakdowns/rate-limiter)

---

#### 5. API Gateway

**What:** Single entry point for all client requests. Handles routing, authentication, rate limiting, request transformation, and monitoring.

**Responsibilities:** Auth, rate limiting, request routing, protocol translation, response caching, logging.

**Interview talking point:** "All client requests hit our API Gateway first. It handles JWT validation, rate limiting, and routes to the appropriate microservice. This decouples clients from internal service topology."

✅ **You're done when:** You can say "single entry point that handles auth, rate limiting, and routing." Mention it in every design, never explain its internals.

---

#### 6. Back-of-Envelope Estimation

**What:** Quick math to estimate scale requirements. Every HLD interview expects this.

**Key numbers to memorize:**

| Metric | Value |
|--------|-------|
| 1 day | 86,400 seconds (~100K) |
| 1 million requests/day | ~12 requests/sec |
| 1 billion requests/day | ~12,000 requests/sec |
| 1 char | 1 byte |
| 1 KB | 1,000 bytes |
| 1 MB | 1,000 KB |
| 1 GB | 1,000 MB |
| 1 TB | 1,000 GB |
| Average tweet | ~300 bytes |
| Average image | ~200 KB |
| Average video (1 min) | ~50 MB |
| SSD read | ~100 μs |
| Memory read | ~100 ns |
| Network round trip (same DC) | ~0.5 ms |
| Network round trip (cross-continent) | ~150 ms |

**Formula template:**
```
Daily Active Users (DAU) × actions/user/day = total daily actions
Total daily actions / 86400 = requests per second (QPS)
Peak QPS = QPS × 2-3 (for spikes)
Storage = actions/day × size per action × retention days
```

**Example:** "If we have 100M DAU, each sending 50 messages/day at 100 bytes each:
- QPS = 5B / 86400 ≈ 58K req/sec, peak ~120K
- Storage/day = 5B × 100B = 500GB/day ≈ 180TB/year"

📺 [ByteByteGo — Back of Envelope](https://www.youtube.com/watch?v=UC5xf8FbdJc)

---

### 1B. Data Storage & Data Modeling

#### 7. SQL vs NoSQL

| | SQL (Relational) | NoSQL |
|---|---|---|
| **Structure** | Fixed schema, tables, rows | Flexible: key-value, document, column-family, graph |
| **Scaling** | Vertical (hard to shard) | Horizontal (built for distribution) |
| **Consistency** | Strong (ACID) | Eventual (BASE), tunable |
| **Best for** | Transactions, complex queries, relationships | High throughput, flexible schema, massive scale |
| **Examples** | PostgreSQL, MySQL, Aurora | DynamoDB, Cassandra, MongoDB, Redis |

**Decision framework for interviews:**
- Need ACID transactions (payments, orders)? → SQL
- Need flexible schema or massive write throughput? → NoSQL
- Need complex joins and aggregations? → SQL
- Need horizontal scaling with simple access patterns? → NoSQL

📺 [ByteByteGo — SQL vs NoSQL](https://www.youtube.com/watch?v=Q2Z6eAGelo4)
📖 [DesignGurus — SQL vs NoSQL](https://www.designgurus.io/answers/detail/sql-vs-nosql)

✅ **You're done when:** Given a use case, you can pick SQL or NoSQL and explain why in one sentence. "Payments need ACID → SQL. Chat messages need high write throughput with simple lookups → NoSQL."

🧪 **Self-test:**
- *User profiles with complex queries (search by name, email, join with orders)?* → SQL — needs joins and flexible queries
- *IoT sensor data: 1M writes/sec, simple key-based reads?* → NoSQL — write throughput + simple access pattern
- *When would you use BOTH in one system?* → Orders in SQL (ACID), product catalog in NoSQL (flexible schema, read-heavy)

---

#### 8. Database Sharding

**What:** Splitting a large database into smaller pieces (shards), each on a separate server.

**Strategies:**
- **Hash-based:** `shard = hash(key) % num_shards`. Even distribution, but adding shards requires rehashing.
- **Range-based:** Shard by ranges (e.g., users A-M on shard 1, N-Z on shard 2). Simple but can create hotspots.
- **Geo-based:** Shard by geography. Good for location-based services.
- **Directory-based:** Lookup table maps keys to shards. Flexible but the directory is a bottleneck.

**Problems:** Cross-shard queries, rebalancing, hotspots, joins across shards.

**Interview talking point:** "I'd shard the messages table by `user_id` using consistent hashing. This ensures even distribution and minimizes data movement when adding new shards."

✅ **You're done when:** You can name 2 strategies (hash-based, range-based), explain consistent hashing in 1 sentence, and know the problems (cross-shard queries, hotspots). You do NOT need to implement a sharding algorithm.

🧪 **Self-test:**
- *You shard users A-M on shard 1, N-Z on shard 2. Users named "Smith" cause hotspots. Why?* → Range-based sharding creates uneven distribution when data isn't uniform
- *You add a 4th shard to a hash-based system with 3 shards. What breaks?* → `hash % 3` ≠ `hash % 4` — most keys remap. Use consistent hashing instead
- *"Get all messages between user A and user B" — they're on different shards. What's the problem?* → Cross-shard query — need to query both shards and merge results

📺 [Gaurav Sen — Database Sharding](https://www.youtube.com/watch?v=5faMjKuB9bc)
📖 [IGotAnOffer — Sharding](https://igotanoffer.com/blogs/tech/sharding-system-design-interview)

---

#### 9. Database Replication

**What:** Maintaining copies of data across multiple servers for availability and read performance.

**Types:**
- **Single-leader (master-slave):** One writer, multiple readers. Simple. Read replicas reduce load.
- **Multi-leader:** Multiple writers. Good for multi-region. Complex conflict resolution.
- **Leaderless:** Any node can accept writes (e.g., Cassandra). Uses quorum reads/writes.

**Interview talking point:** "For our read-heavy service (100:1 read-write ratio), I'd use single-leader replication with 3 read replicas. Writes go to the leader, reads are distributed across replicas. We accept slight replication lag (eventual consistency) for reads."

✅ **You're done when:** You know single-leader vs multi-leader vs leaderless, can pick one for a given use case, and can explain replication lag. Skip multi-leader internals.

📺 [ByteByteGo — Database Replication](https://www.youtube.com/watch?v=bI8Ry6GhMSE)

---

#### 10. Database Indexing

**What:** Data structures that speed up read queries at the cost of slower writes and extra storage.

**Types:**
- **B-Tree:** Default for most SQL databases. Good for range queries and point lookups.
- **LSM Tree:** Used by Cassandra, RocksDB. Optimized for write-heavy workloads.
- **Inverted Index:** Maps terms to documents. Used by search engines (Elasticsearch).
- **Geospatial Index:** R-tree, Quadtree, Geohash. For location queries.

**Interview talking point:** "I'd add a composite index on `(user_id, created_at)` for the messages table so we can efficiently query 'get latest 50 messages for user X'."

✅ **You're done when:** You can say "B-Tree for general queries, LSM for write-heavy, inverted index for search." Know when to add a composite index. Skip internal data structure details.

---

#### 11. ACID vs BASE

| ACID (SQL) | BASE (NoSQL) |
|------------|-------------|
| **A**tomicity — all or nothing | **B**asically **A**vailable — system always responds |
| **C**onsistency — valid state transitions | **S**oft state — state may change over time |
| **I**solation — concurrent txns don't interfere | **E**ventual consistency — will converge eventually |
| **D**urability — committed data survives crashes | |

**When to mention:** Payment systems need ACID. Social media feeds can use BASE.

✅ **You're done when:** You can say "ACID for transactions where correctness matters, BASE for high-availability systems where slight staleness is OK." That's it.

🧪 **Self-test:**
- *E-commerce: order placement needs ACID or BASE?* → ACID — partial order = lost money
- *Social media: "likes" count needs ACID or BASE?* → BASE — showing 999 instead of 1000 likes for 2 seconds is fine

---

#### 12. Database Isolation Levels

**What:** Isolation levels define how concurrent transactions interact with each other. This is the "I" in ACID — and interviewers drill into it during payment, booking, and e-commerce deep dives.

| Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads | Performance |
|-------|------------|---------------------|---------------|-------------|
| **Read Uncommitted** | ✅ Possible | ✅ Possible | ✅ Possible | Fastest |
| **Read Committed** | ❌ Prevented | ✅ Possible | ✅ Possible | Fast |
| **Repeatable Read** | ❌ Prevented | ❌ Prevented | ✅ Possible | Moderate |
| **Serializable** | ❌ Prevented | ❌ Prevented | ❌ Prevented | Slowest |

**Key terms:**
- **Dirty Read:** Reading data written by an uncommitted transaction (it might roll back).
- **Non-Repeatable Read:** Reading the same row twice in a transaction and getting different values (another txn committed between reads).
- **Phantom Read:** A query returns different rows when re-executed (another txn inserted/deleted rows matching the WHERE clause).

**Practical defaults:**
- PostgreSQL: Read Committed (default)
- MySQL InnoDB: Repeatable Read (default)
- DynamoDB: No traditional isolation — uses optimistic locking with conditional writes

**Interview talking point:** "For the booking system, I'd use Serializable isolation on the seat reservation transaction to prevent double-booking. For the product catalog reads, Read Committed is sufficient since slight staleness is acceptable. The trade-off is that Serializable is slower due to locking, so we only use it on the critical payment/booking path."

**When interviewers ask "How do you prevent double-booking?":**
1. Pessimistic locking: `SELECT ... FOR UPDATE` (locks the row)
2. Optimistic locking: Version column — read version, attempt update with `WHERE version = X`, retry on conflict
3. Serializable isolation: Database guarantees no concurrent conflicts
4. Distributed lock: Redis/ZooKeeper lock on the resource ID

---

#### 13. CAP Theorem

**What:** In a distributed system experiencing a network partition, you must choose between Consistency and Availability. You cannot have both.

- **C (Consistency):** Every read gets the most recent write.
- **A (Availability):** Every request gets a response (even if stale).
- **P (Partition Tolerance):** System works despite network failures between nodes.

**Reality:** P is not optional in distributed systems. So the real choice is CP vs AP.

| Choice | Behavior During Partition | Example |
|--------|--------------------------|---------|
| **CP** | Rejects requests to maintain consistency | HBase, MongoDB (default), ZooKeeper |
| **AP** | Serves potentially stale data | Cassandra, DynamoDB (default), CouchDB |

**Note:** DynamoDB is tunable — you can request strongly consistent reads, but the default is eventually consistent (AP behavior).

**Interview talking point:** "For a chat system, I'd choose AP — it's better to show a slightly stale message list than to show nothing. For a payment system, I'd choose CP — we can't risk inconsistent balances."

✅ **You're done when:** You can explain "P is not optional, so the real choice is CP vs AP" and pick the right one for a given use case. Skip the mathematical proof.

🧪 **Self-test:**
- *Your chat app partitions. Users see stale message lists but the app stays up. CP or AP?* → AP — availability over consistency
- *Bank transfer during a partition. You reject the request. CP or AP?* → CP — consistency over availability
- *DynamoDB default is AP. How do you get strong consistency?* → Request strongly consistent reads (costs 2x RCU)
- *"Why not just pick CA and skip partitions?"* → Partitions are inevitable in distributed systems. CA only works on a single node.

📺 [ByteByteGo — CAP Theorem](https://www.youtube.com/watch?v=_RbsFXWRZ10)
📖 [HelloInterview — CAP Theorem](https://www.hellointerview.com/learn/system-design/deep-dives/cap-theorem)

---

#### 14. Consistent Hashing

**What:** A hashing technique where adding/removing servers only requires remapping `K/N` keys (K = total keys, N = total servers), instead of rehashing everything.

**How:** Servers and keys are placed on a virtual ring. Each key is assigned to the next server clockwise on the ring. Virtual nodes (vnodes) ensure even distribution.

**Used in:** DynamoDB, Cassandra, load balancers, distributed caches.

**Interview talking point:** "For our distributed cache, I'd use consistent hashing with 150 virtual nodes per server. When we add a new cache server, only ~1/N of keys need to be remapped."

✅ **You're done when:** You can explain "keys and servers on a ring, each key goes to the next server clockwise, virtual nodes ensure even distribution, adding a server only remaps 1/N keys." Skip the hash function math.

📺 [Gaurav Sen — Consistent Hashing](https://www.youtube.com/watch?v=zaRkONvyGr8)

---

#### 15. Time-Series Databases

**What:** Optimized for timestamped data — metrics, IoT sensor data, stock prices, logs.

**Examples:** InfluxDB, TimescaleDB, Amazon Timestream, Prometheus.

**When to use:** Monitoring systems, real-time analytics, stock trackers, IoT platforms.

---

#### 16. Graph Databases

**What:** Store data as nodes and edges. Optimized for relationship-heavy queries.

**Examples:** Neo4j, Amazon Neptune, JanusGraph.

**When to use:** Social graphs (friend-of-friend queries), recommendation engines, fraud detection.

---

#### 17. Object/Blob Storage

**What:** Storage for unstructured data — images, videos, files, backups.

**Examples:** Amazon S3, Google Cloud Storage, Azure Blob Storage.

**Key features:** Virtually unlimited scale, high durability (11 nines), cheap for large files.

**Interview talking point:** "User-uploaded images go to S3 with a CDN in front. We store only the S3 key in our metadata database, not the file itself."

---

#### 18. Data Modeling & Schema Design

**What:** The process of designing your database schema driven by access patterns, not entity relationships. This is the #1 skill interviewers test when you say "define data model" — and most candidates fumble it.

**The Rule:** In SQL, model your entities first, then optimize. In NoSQL, start with your access patterns, then design the schema to serve them.

**SQL Modeling Checklist:**
- Identify entities and relationships (1:1, 1:N, M:N)
- Normalize to 3NF first, then selectively denormalize for read performance
- Add indexes based on query patterns: `CREATE INDEX ON messages(user_id, created_at DESC)`
- Use composite indexes for multi-column WHERE + ORDER BY queries

**NoSQL / DynamoDB Modeling Checklist:**
- List ALL access patterns before designing a single table
- Choose partition key (PK) for even distribution + query isolation
- Choose sort key (SK) for range queries within a partition
- Use Global Secondary Indexes (GSI) for alternate access patterns
- Consider single-table design: store multiple entity types in one table using PK/SK prefixes

**Example — Chat App DynamoDB Design:**
```
Access patterns:
1. Get messages for a conversation (sorted by time)
2. Get all conversations for a user
3. Get user profile

Table design:
PK              | SK                    | Data
USER#alice      | PROFILE               | {name, avatar, status}
USER#alice      | CONV#conv123          | {lastMessage, unread}
CONV#conv123    | MSG#2026-03-18T10:00  | {sender, text, type}
CONV#conv123    | MSG#2026-03-18T10:01  | {sender, text, type}

GSI1: SK as PK (to query all messages in a conversation)
```

**When to Denormalize:**
- Read:write ratio > 10:1 → denormalize (duplicate data for faster reads)
- Data changes rarely but is read constantly → denormalize
- Need sub-millisecond reads → denormalize into cache-friendly structures
- Data changes frequently and consistency matters → keep normalized

**Interview talking point:** "Before choosing a schema, I'd list the top 5 access patterns. For this chat system, the primary pattern is 'get latest 50 messages for conversation X,' so I'd partition by conversation_id and sort by timestamp. For DynamoDB, that's PK=CONV#id, SK=MSG#timestamp."

---

### 1C. Caching

#### 19. Caching Strategies

| Strategy | How It Works | Best For |
|----------|-------------|----------|
| **Cache-Aside (Lazy)** | App checks cache → miss → read DB → write to cache | Read-heavy, general purpose |
| **Read-Through** | Cache itself fetches from DB on miss | Simplifies app code |
| **Write-Through** | Write to cache AND DB simultaneously | Strong consistency needed |
| **Write-Behind (Write-Back)** | Write to cache, async flush to DB | Write-heavy, can lose data |
| **Write-Around** | Write directly to DB, skip cache | Infrequently read data |

**Most common in interviews:** Cache-aside. "App checks Redis first. On miss, query DB, then populate Redis with a TTL of 5 minutes."

✅ **You're done when:** You can explain cache-aside in detail and name the other 4 strategies. You do NOT need to implement any of them. Default answer: "Cache-aside with Redis and a 5-minute TTL."

🧪 **Self-test:**
- *Write-behind cache: the app crashes before async flush to DB. What happens?* → Data loss — the write was only in cache. This is the trade-off for write-heavy performance.
- *Your cache TTL is 5 min. A user updates their profile. They refresh and see old data. Why?* → Stale cache. Fix: invalidate cache on write (write-through) or accept staleness.
- *When would you pick write-through over cache-aside?* → When you need strong consistency between cache and DB (e.g., inventory counts during checkout)

📺 [ByteByteGo — Caching Strategies](https://www.youtube.com/watch?v=ccemOqDrc2I)
📖 [IGotAnOffer — Caching](https://igotanoffer.com/blogs/tech/caching-system-design-interview)

---

#### 20. Cache Eviction Policies

- **LRU (Least Recently Used):** Evict the item not accessed for the longest time. Most common.
- **LFU (Least Frequently Used):** Evict the item accessed the fewest times.
- **TTL (Time-To-Live):** Items expire after a set duration.
- **FIFO:** First in, first out.

**Interview default:** "LRU with a TTL of 5-15 minutes covers most cases."

✅ **You're done when:** You can say "LRU with TTL" as your default and explain why LRU works (hot data stays, cold data evicts). Know LFU exists. That's it.

---

#### 21. Redis vs Memcached

| | Redis | Memcached |
|---|---|---|
| Data structures | Strings, lists, sets, sorted sets, hashes | Strings only |
| Persistence | Optional (RDB/AOF) | None |
| Replication | Built-in | None |
| Use case | Cache + data store, leaderboards, sessions, pub/sub | Pure caching, simple key-value |

**Interview default:** Use Redis unless you have a specific reason for Memcached.

✅ **You're done when:** You can say "Redis — it supports data structures beyond strings, has persistence, and handles pub/sub. Memcached only if you need pure simple caching with multi-threaded performance."

---

#### 22. CDN (Content Delivery Network)

**What:** Geographically distributed network of servers that cache and serve static content (images, videos, CSS, JS) close to users.

**How:** User requests content → DNS routes to nearest CDN edge → if cached, serve immediately → if not, fetch from origin server, cache, then serve.

**Types:**
- **Push CDN:** You upload content to CDN proactively. Good for static content that changes rarely.
- **Pull CDN:** CDN fetches from origin on first request. Good for dynamic content.

**Interview talking point:** "All static assets (images, videos) are served through a CDN. This reduces latency for global users and offloads traffic from our origin servers."

📺 [ByteByteGo — CDN](https://www.youtube.com/watch?v=RI9np1LWzqw)

---

#### 23. Multi-Layer Caching

**Layers (from closest to user to farthest):**
1. **Browser/Client cache** — HTTP cache headers (Cache-Control, ETag)
2. **CDN cache** — Edge servers worldwide
3. **API Gateway cache** — Cache frequent API responses
4. **Application cache** — Redis/Memcached in front of DB
5. **Database cache** — Query cache, buffer pool

**Interview talking point:** "We use multi-layer caching: CDN for static assets, Redis for hot data (user profiles, feed), and DB query cache for complex aggregations."

---

#### 24. Hot Key Problem & Cache Warming

**Hot Key Problem:** When a single cache key receives disproportionate traffic (e.g., a viral tweet, a celebrity profile), that single Redis node becomes a bottleneck.

**Solutions:**
- **Key replication:** Replicate hot key across N cache nodes with suffix: `key_1`, `key_2`, ... `key_N`. Client randomly picks one. Spreads load across nodes.
- **Local cache + distributed cache:** Keep a short-TTL (5-10s) local in-memory cache (Caffeine/Guava) in front of Redis. Most requests never hit Redis.
- **Read replicas:** Use Redis read replicas for hot keys. Writes go to primary, reads distributed across replicas.

**Cache Warming:** Pre-populating cache before traffic hits it. Critical after:
- New deployment (fresh cache nodes = 100% miss rate = DB gets slammed)
- Cache node replacement
- Disaster recovery failover

**Warming strategies:**
- **Startup warming:** On boot, query DB for top N most-accessed keys and pre-load into cache
- **Shadow traffic:** Replay recent production read traffic against new cache nodes before routing live traffic
- **Lazy warming with protection:** Accept cold cache but use a mutex/lock so only ONE request per key hits DB on miss (others wait or get stale data)

#### 25. Cache Invalidation Patterns

> "There are only two hard things in CS: cache invalidation and naming things." — Phil Karlton. Interviewers love drilling here.

| Pattern | How It Works | Pros | Cons |
|---------|-------------|------|------|
| **TTL-based** | Key expires after N seconds | Simple, no coordination needed | Stale data until TTL expires |
| **Event-driven** | DB write → publish event → consumer invalidates cache | Near-real-time freshness | Requires event infrastructure (Kafka/SNS) |
| **Write-through invalidation** | App updates cache AND DB on every write | Always fresh | Higher write latency |
| **Version-based** | Store version number with cached data. On read, compare with DB version | Precise invalidation | Extra DB read for version check |

**Interview default:** "TTL-based for most data (5-15 min). Event-driven invalidation for data where staleness is unacceptable (inventory, pricing). We publish a `CacheInvalidation` event to Kafka on every write, and cache consumers evict the key."

**The Read-Your-Writes Problem:** User updates profile → refreshes page → sees OLD profile (cache hasn't expired yet). Solutions:
1. Invalidate cache on write (event-driven)
2. Read from leader/primary DB for the writing user for N seconds after their write
3. Write-through cache (update cache + DB together)

---

### 1D. Communication & Networking

#### 26. DNS & How It Works (Foundation for Every Global System Design)

**What:** Domain Name System — translates human-readable domain names (google.com) to IP addresses (142.250.80.46). Every system design that mentions "global users," "CDN," or "multi-region" depends on DNS.

**How DNS resolution works (simplified):**
```
Browser → Local DNS Cache → Recursive Resolver (ISP) → Root Server (.com)
    → TLD Server (google.com) → Authoritative Server → IP Address
    ← cached at each layer with TTL
```

**DNS-based load balancing (Route 53 / Cloud DNS):**

| Routing Policy | How It Works | Use Case |
|---------------|-------------|----------|
| **Simple** | Returns one IP | Single server |
| **Weighted** | Distributes traffic by weight (70/30) | Canary deployments, A/B testing |
| **Latency-based** | Routes to lowest-latency region | Global apps — user in Tokyo → ap-northeast-1 |
| **Geolocation** | Routes by user's geographic location | Compliance (EU data stays in EU), localized content |
| **Failover** | Primary + secondary. Switches on health check failure | Disaster recovery |

**Limitations of DNS load balancing:**
- TTL caching means changes propagate slowly (minutes to hours)
- No health checks at DNS level (need separate health check service)
- Client-side caching can ignore TTL — stale DNS entries persist

**Interview talking point:** "For global routing, I'd use Route 53 with latency-based routing so users hit the nearest region. For disaster recovery, failover routing with health checks automatically redirects traffic if a region goes down. DNS TTL of 60 seconds balances freshness with DNS query load."

**Real-world reference:** Facebook's 2021 outage was caused by a BGP/DNS misconfiguration that made their DNS servers unreachable — the entire platform went down for 6 hours. This is why DNS redundancy and monitoring matter.

---

#### 27. REST vs gRPC vs GraphQL

| | REST | gRPC | GraphQL |
|---|---|---|---|
| Protocol | HTTP/1.1 | HTTP/2 | HTTP |
| Format | JSON | Protobuf (binary) | JSON |
| Speed | Moderate | Fast (binary, multiplexed) | Moderate |
| Best for | Public APIs, CRUD | Internal microservice-to-microservice | Mobile apps, flexible queries |
| Streaming | No (needs WebSocket) | Yes (bidirectional) | Subscriptions |

**Interview default:** REST for external APIs, gRPC for internal service communication.

✅ **You're done when:** You can pick REST vs gRPC vs GraphQL for a given scenario and say why in one sentence. Don't memorize protocol internals.

---

#### 28. Sync vs Async Communication

- **Synchronous:** Caller waits for response. HTTP request-response. Simple but creates coupling.
- **Asynchronous:** Caller sends message and moves on. Uses message queues. Decoupled, resilient.

**When to use async:** Email sending, video processing, notification delivery, any non-time-critical operation.

---

#### 29. WebSockets vs Long Polling vs SSE

| | WebSocket | Long Polling | SSE (Server-Sent Events) |
|---|---|---|---|
| Direction | Bidirectional | Client-initiated | Server → Client only |
| Connection | Persistent | Repeated HTTP | Persistent HTTP |
| Use case | Chat, gaming, live collab | Fallback for WebSocket | Live feeds, notifications |
| Overhead | Low after handshake | High (repeated connections) | Low |

**Interview talking point:** "For real-time chat, I'd use WebSockets for bidirectional communication. For a live news feed where the server pushes updates, SSE is simpler and sufficient."

📺 [IGotAnOffer — Polling, SSE, WebSockets](https://igotanoffer.com/blogs/tech/polling-sse-websockets-system-design-interview)

---

#### 30. WebSocket Connection Management at Scale

**What:** Managing millions of concurrent WebSocket connections requires a dedicated architecture layer. This is a guaranteed deep-dive when you propose WebSockets for any real-time system.

**Key challenges:**
- **Connection limits:** A single server can handle ~65K connections (limited by ports). For 10M concurrent users, you need ~150+ connection servers.
- **Sticky sessions:** Once a WebSocket is established, all messages for that connection must route to the same server. Use consistent hashing on `user_id` at the load balancer.
- **Heartbeat/Keepalive:** Clients send periodic pings (every 30s). If no pong received within timeout, server closes connection and cleans up resources.
- **Reconnection:** Clients implement exponential backoff reconnection (1s, 2s, 4s, 8s...) with jitter. On reconnect, client sends `last_received_message_id` to get missed messages.

**Architecture pattern:**
```
Clients → L4 Load Balancer (sticky by user_id)
              ↓
         Connection Servers (stateful — hold WebSocket connections)
              ↓
         Message Router (Redis Pub/Sub or Kafka)
              ↓
         Application Servers (stateless — business logic)
```

**Why separate connection servers from app servers?**
- Connection servers are stateful (hold open sockets) — scale based on connection count
- App servers are stateless (process messages) — scale based on message throughput
- Decoupling lets you scale each independently

**Interview talking point:** "I'd separate connection management from business logic. Dedicated connection servers hold WebSocket connections, each handling ~50K concurrent connections. When user A sends a message to user B, the message goes to the app server for processing, then the message router (Redis Pub/Sub) fans it out to whichever connection server holds user B's socket. Clients implement heartbeats every 30 seconds and exponential backoff reconnection."

---

#### 31. Message Queues

**What:** Asynchronous communication between services. Producer sends message to queue, consumer processes it independently.

**Examples:** Apache Kafka, RabbitMQ, Amazon SQS, Redis Streams.

| | Kafka | RabbitMQ | SQS |
|---|---|---|---|
| Model | Distributed log (pub/sub) | Traditional message broker | Managed queue |
| Ordering | Per-partition | Per-queue | Best-effort (FIFO available) |
| Throughput | Very high (millions/sec) | Moderate | Moderate |
| Retention | Configurable (days/weeks) | Until consumed | 14 days max |
| Best for | Event streaming, logs, analytics | Task queues, RPC | Simple async tasks on AWS |

**Interview talking point:** "When a user posts content, we publish an event to Kafka. Separate consumers handle feed generation, notification sending, and analytics — all independently and at their own pace."

✅ **You're done when:** You can pick Kafka vs SQS vs RabbitMQ for a use case ("Kafka for event streaming with replay, SQS for simple async tasks, RabbitMQ for complex routing") and explain producer → queue → consumer flow.

📺 [ByteByteGo — Message Queues](https://www.youtube.com/watch?v=W4_aGb_MOls)
📖 [IGotAnOffer — Queues & Pub/Sub](https://igotanoffer.com/blogs/tech/queues-pubsub-system-design-interview)

---

#### 32. Pub/Sub Pattern

**What:** Publishers emit events to topics. Subscribers listen to topics they care about. Publishers don't know about subscribers.

**Difference from Queue:** In a queue, each message is consumed by ONE consumer. In pub/sub, each message goes to ALL subscribers.

**Examples:** Kafka topics, Google Pub/Sub, Amazon SNS, Redis Pub/Sub.

---

#### 33. Event-Driven Architecture

**What:** Services communicate by producing and consuming events rather than direct API calls.

**Benefits:** Loose coupling, independent scaling, better fault isolation, natural audit trail.

**Pattern:** Service A emits "OrderCreated" event → Kafka → Service B (inventory), Service C (notification), Service D (analytics) all consume independently.

---

#### 34. Service Discovery

**What:** How services find each other in a microservices architecture where instances are dynamic.

**Approaches:**
- **Client-side discovery:** Client queries a registry (e.g., Eureka) and picks an instance.
- **Server-side discovery:** Load balancer queries registry and routes. Client just hits the LB.
- **DNS-based:** Services register DNS entries. Simple but slow to update.

---

### 1E. Distributed Systems Concepts

#### 35. Consensus Algorithms (Raft, Paxos)

> 🔶 **STRETCH** — SDE2 is never asked to explain Raft/Paxos internals. Just know: "We'd use ZooKeeper or etcd for leader election and distributed coordination."

**What:** Protocols that allow distributed nodes to agree on a single value even if some nodes fail. Raft uses leader election + log replication. Majority (quorum) must agree.

**When to mention:** Leader election, distributed config stores. One sentence: "Leader election is handled by ZooKeeper using a consensus protocol."

---

#### 36. Leader Election

**What:** Process of designating one node as the coordinator (leader) among a group of nodes.

**Used in:** Database replication (who accepts writes), distributed locks, task scheduling.

**Tools:** ZooKeeper, etcd, Consul.

📖 [IGotAnOffer — Leader Election](https://igotanoffer.com/blogs/tech/leader-election-system-design-interview)

---

#### 37. Distributed Locking

**What:** Ensuring only one process across multiple servers can access a shared resource at a time.

**Implementations:** Redis (Redlock algorithm), ZooKeeper, DynamoDB conditional writes.

**Interview talking point:** "To prevent double-booking a ticket, I'd use a distributed lock via Redis with a TTL. The service acquires the lock on `seat_id`, processes the booking, then releases it."

---

#### 38. Idempotency

**What:** An operation that produces the same result regardless of how many times it's executed.

**Why it matters:** Network retries can cause duplicate requests. Without idempotency, you might charge a user twice.

**Implementation:** Client sends a unique `idempotency_key` with each request. Server checks if this key was already processed before executing.

**Interview talking point:** "All our payment APIs are idempotent. The client generates a UUID as an idempotency key. If we receive a duplicate, we return the cached response."

✅ **You're done when:** You can explain "client sends unique key, server checks if already processed, returns cached result if yes." Mention it in every write-path design. One of the highest-signal concepts at SDE2.

🧪 **Self-test:**
- *User clicks "Pay" twice due to slow network. Without idempotency, what happens?* → Double charge
- *Where do you store the idempotency key lookup — Redis or DB?* → Redis (fast lookup, TTL for auto-cleanup). DB as fallback for durability.
- *GET requests are naturally idempotent. Why?* → They don't modify state — calling GET /users/123 ten times returns the same result

---

#### 39. Eventual vs Strong Consistency

- **Strong:** After a write, all subsequent reads return the updated value. Higher latency.
- **Eventual:** After a write, reads may return stale data for a short period. Lower latency, higher availability.

**Decision framework:**
- User profile updates → eventual consistency is fine
- Bank balance → strong consistency required
- Social media feed → eventual consistency is fine
- Inventory count during checkout → strong consistency required

---

#### 40. Data Partitioning Strategies

Same as sharding (covered in #8), but also applies to:
- **Kafka topics:** Partition by key for ordering guarantees
- **Cache clusters:** Partition by consistent hashing
- **Search indices:** Partition by document ID or time range

---

#### 41. Replication Strategies

- **Synchronous:** Leader waits for replica ACK before confirming write. Strong consistency, higher latency.
- **Asynchronous:** Leader confirms write immediately, replicates in background. Lower latency, risk of data loss.
- **Semi-synchronous:** Wait for at least 1 replica ACK. Balance of both.

---

#### 42. Conflict Resolution

**When:** Multi-leader or leaderless replication where concurrent writes to same key happen.

**SDE2 level — know these two:**
- **Last-Write-Wins (LWW):** Timestamp-based. Simple but can lose data. **Default answer for SDE2.**
- **Application-level:** Let the user resolve (e.g., Google Docs shows both versions).

> 🔶 **STRETCH:** Vector Clocks (track causal ordering) and CRDTs (auto-merging data structures) are SDE3 territory. At SDE2, just say "LWW with timestamp" and move on.

---

#### 43. Circuit Breaker Pattern

**What:** Prevents a service from repeatedly calling a failing downstream service. After N failures, the circuit "opens" and requests fail fast without calling the downstream.

**States:** Closed (normal) → Open (failing fast) → Half-Open (testing recovery).

**Interview talking point:** "If our payment service is down, the circuit breaker opens after 5 consecutive failures. For 30 seconds, all payment requests fail fast with a friendly error. Then it tries one request — if it succeeds, the circuit closes."

✅ **You're done when:** You can explain the 3 states (Closed → Open → Half-Open) and when to mention it ("downstream service might fail"). One sentence in your design is enough.

---

#### 44. Retry with Exponential Backoff

**What:** On failure, retry with increasing delays: 1s, 2s, 4s, 8s... plus random jitter to prevent thundering herd.

**Formula:** `delay = min(base * 2^attempt + random_jitter, max_delay)`

**Interview talking point:** "Failed API calls are retried with exponential backoff starting at 100ms, capped at 30 seconds, with random jitter to avoid synchronized retries."

✅ **You're done when:** You can say "exponential backoff with jitter" and explain why jitter matters (prevents thundering herd). The formula is nice-to-know, not need-to-know.

---

#### 45. Bloom Filters

**What:** Space-efficient probabilistic data structure. Tells you "definitely not in set" or "probably in set." False positives possible, false negatives impossible.

**SDE2 level:** Know what it does and when to mention it. Don't memorize the math.

**When to mention:** "Before querying the DB, we check a Bloom filter to eliminate unnecessary lookups." That's the full answer at SDE2.

---

#### 46. Multi-Region Architecture

> 🔶 **STRETCH for SDE2** — You are NOT penalized for missing multi-region details. At SDE2, mention it as a "future extension" in your wrap-up. Only go deeper if the interviewer explicitly asks "How would you make this global?"

**SDE2 minimum:** Know the 3 patterns by name, mention CDN + geo-routing. That's it.

**Patterns (know the names):**
- **Active-Passive:** One region serves traffic, standby takes over on failure. Simple.
- **Active-Active:** All regions serve traffic. Complex (needs conflict resolution).
- **Read-local, Write-global:** Reads from nearest region, writes to single primary.

**If interviewer asks "How would you make this global?" (your 30-second answer):**
1. Add geo-routing at DNS/LB layer (Route 53)
2. CDN for static content
3. Replicate data across regions (DynamoDB Global Tables or Aurora Global DB)
4. Use Last-Write-Wins for conflict resolution (simplest)

---

#### 47. Merkle Trees *(🔶 STRETCH — rarely asked at SDE2)*

**What:** Hash tree to verify data consistency between replicas. Comparing root hashes tells you instantly if replicas are in sync. Used in DynamoDB, Git, Cassandra. Just know it exists — don't prep this.

---

#### 48. Distributed Transactions (Saga, Outbox, Compensating Transactions)

> This is asked in 30-40% of Amazon/Uber/Stripe SDE2 rounds — any time you design a payment, booking, or e-commerce system that spans multiple services.

**The Problem:** In microservices, a single business operation (e.g., "place an order") spans multiple services (Order Service, Payment Service, Inventory Service). You can't use a single DB transaction across services.

**Two-Phase Commit (2PC) — Know Why It's Bad:**
- Coordinator asks all participants to prepare → all say "yes" → coordinator says "commit"
- Problem: If coordinator crashes after prepare, all participants are stuck holding locks. Blocking protocol. Don't use it in microservices.

**Saga Pattern — The Standard Answer:**

A saga is a sequence of local transactions. Each service does its own transaction and publishes an event. If any step fails, compensating transactions undo previous steps.

| Approach | How It Works | Pros | Cons |
|----------|-------------|------|------|
| **Choreography** | Each service listens for events and reacts | Decoupled, simple for 2-3 steps | Hard to track, spaghetti for 5+ steps |
| **Orchestration** | Central orchestrator tells each service what to do | Clear flow, easy to monitor | Orchestrator is a SPOF, coupling |

**Example — Order Saga (Orchestration):**
```
1. Order Service: Create order (PENDING)
2. Payment Service: Charge customer → success
3. Inventory Service: Reserve items → success
4. Order Service: Update order (CONFIRMED)

If step 3 fails:
3a. Payment Service: REFUND customer (compensating transaction)
3b. Order Service: Update order (CANCELLED)
```

**Outbox Pattern — Reliable Event Publishing:**
- Problem: Service writes to DB AND publishes event to Kafka. If Kafka publish fails after DB commit, data is inconsistent.
- Solution: Write the event to an "outbox" table in the SAME DB transaction as the business data. A separate process (CDC or poller) reads the outbox and publishes to Kafka.
```
BEGIN TRANSACTION
  INSERT INTO orders (id, status) VALUES (123, 'CREATED');
  INSERT INTO outbox (event_type, payload) VALUES ('OrderCreated', '{...}');
COMMIT
-- Separate CDC process reads outbox → publishes to Kafka → marks as sent
```

**Interview talking point:** "For the order flow spanning Payment and Inventory services, I'd use the Saga pattern with orchestration. The Order Service acts as the orchestrator. If payment succeeds but inventory reservation fails, we execute a compensating transaction to refund the payment. To ensure reliable event publishing, I'd use the Outbox pattern — the event is written to an outbox table in the same DB transaction, then a CDC process publishes it to Kafka."

✅ **You're done when:** You can explain Saga (choreography vs orchestration), give a compensating transaction example, and describe the Outbox pattern. Mention this in EVERY payment/booking/e-commerce design.

---

#### 49. Change Data Capture (CDC)

**What:** Capturing row-level changes (INSERT, UPDATE, DELETE) from a database and streaming them as events. The modern answer to "how do you keep two data stores in sync."

**How it works:** CDC reads the database's transaction log (WAL in PostgreSQL, binlog in MySQL) and publishes each change as an event to Kafka.

**Tools:** Debezium (open-source, most popular), DynamoDB Streams, AWS DMS, Maxwell.

**Use cases (mention these in interviews):**
- **Search indexing:** DB change → Kafka → Elasticsearch consumer updates search index
- **Cache invalidation:** DB change → Kafka → consumer invalidates/updates Redis cache
- **Analytics pipeline:** DB change → Kafka → data warehouse (real-time analytics)
- **Outbox pattern:** CDC reads outbox table → publishes events to Kafka (no polling needed)
- **Cross-service sync:** Service A's DB changes → Kafka → Service B consumes and updates its own DB

**Interview talking point:** "To keep Elasticsearch in sync with our primary DB, I'd use CDC via Debezium. It reads the PostgreSQL WAL and publishes change events to Kafka. An Elasticsearch consumer processes these events and updates the search index. This is more reliable than dual-writes (writing to both DB and ES) because CDC guarantees we capture every change, even if the application crashes mid-operation."

**Why CDC over dual-writes?** Dual-writes (app writes to DB AND Elasticsearch) can fail halfway — DB succeeds but ES fails, leaving them out of sync. CDC reads from the DB's transaction log, so it captures every committed change regardless of application failures.

---

### 1F. Reliability & Observability

#### 50. Single Point of Failure (SPOF)

**What:** Any component whose failure brings down the entire system.

**How to eliminate:** Redundancy at every layer — multiple app servers, DB replicas, multi-AZ deployment, redundant load balancers.

**Interview talking point:** "I'd deploy across 3 availability zones. Each component — load balancer, app servers, database — has redundant instances. No single failure takes down the system."

---

#### 51. Failover Mechanisms

- **Active-Passive:** Standby takes over when primary fails. Simple, some downtime during switchover.
- **Active-Active:** Both handle traffic. Zero downtime. More complex (data sync needed).

---

#### 52. Health Checks & Heartbeats

- **Health checks:** Load balancer periodically pings servers. Unhealthy servers are removed from rotation.
- **Heartbeats:** Nodes send periodic signals to a coordinator. Missing heartbeats trigger failover.

---

#### 53. Monitoring, Logging, Alerting

**The three pillars of observability:**
1. **Metrics:** Numeric measurements over time (QPS, latency p99, error rate, CPU). Tools: Prometheus, CloudWatch, Datadog.
2. **Logs:** Detailed event records. Tools: ELK stack, Splunk, CloudWatch Logs.
3. **Traces:** Request flow across services. Tools: Jaeger, X-Ray, Zipkin.

**Interview talking point:** "I'd monitor p99 latency, error rate, and QPS with dashboards and alerts. If p99 > 500ms or error rate > 1%, we get paged."

✅ **You're done when:** You can say "metrics (p99, error rate, QPS), logs (ELK/CloudWatch), traces (Jaeger/X-Ray)" and mention monitoring in your wrap-up. Don't explain how Prometheus works internally.

---

#### 54. Distributed Tracing

**What:** Tracking a single request as it flows through multiple microservices. Each service adds a span to the trace.

**How:** A unique trace ID is generated at the entry point and propagated through all downstream calls via headers.

---

#### 55. SLIs, SLOs, SLAs & Error Budgets

**What:** The framework for defining, measuring, and committing to reliability. Every SDE2 should be able to define these for any system they design.

| Term | Definition | Example |
|------|-----------|---------|
| **SLI** (Service Level Indicator) | A quantitative measure of service behavior | p99 latency of the read API, error rate, throughput |
| **SLO** (Service Level Objective) | A target value for an SLI | "99.9% of reads complete in <200ms" |
| **SLA** (Service Level Agreement) | A contract with consequences if SLO is breached | "99.95% uptime or customer gets service credits" |
| **Error Budget** | The allowed amount of unreliability: `100% - SLO` | SLO = 99.9% → error budget = 0.1% = ~43 min downtime/month |

**How to use error budgets:**
- Budget healthy (>50% remaining) → ship features aggressively, run experiments
- Budget low (<25% remaining) → slow down deployments, focus on reliability
- Budget exhausted → freeze all non-critical changes, prioritize stability

**Common SLIs by system type:**
- **API service:** p50/p99 latency, error rate (5xx), availability
- **Data pipeline:** end-to-end processing latency, data freshness, completeness
- **Storage system:** durability, read/write latency, availability

**Interview talking point:** "For this service, I'd define three SLIs: availability (target 99.95%), p99 read latency (target <200ms), and error rate (target <0.1%). Our SLO gives us a monthly error budget of ~22 minutes of downtime. If we burn through 50% of the budget in the first week, we'd freeze deployments and investigate."

---

#### 56. Deployment Patterns & Operational Resilience

> These show operational maturity. Amazon interviewers evaluate this under "Ownership" LP. Mention at least one in your wrap-up.

**Deployment Strategies:**

| Strategy | How It Works | Risk | Rollback Speed |
|----------|-------------|------|----------------|
| **Blue-Green** | Two identical environments. Route traffic from blue (old) to green (new) instantly | Low — instant rollback by switching back | Seconds |
| **Canary** | Deploy to 1-5% of traffic first. Monitor. Gradually increase to 100% | Very low — blast radius is small | Seconds (route canary back) |
| **Rolling** | Replace instances one at a time. Old and new versions coexist temporarily | Medium — mixed versions during rollout | Minutes |
| **Feature Flags** | Deploy code to 100% but toggle feature on for subset of users | Very low — disable flag instantly | Instant (flip flag) |

**Interview talking point:** "I'd deploy using canary releases — push to 2% of traffic, monitor p99 latency and error rate for 15 minutes, then gradually roll to 25%, 50%, 100%. If error rate exceeds 1% at any stage, auto-rollback."

**Load Shedding — When Everything Is On Fire:**
- When system is overloaded, intentionally drop low-priority requests to protect high-priority ones
- Example: During a flash sale, shed "browse recommendations" requests to protect "checkout" requests
- Implementation: Priority queue at the API Gateway. When queue depth > threshold, reject low-priority requests with 503

**Chaos Engineering (Know the Concept):**
- Intentionally inject failures in production to verify resilience: kill random instances, add network latency, corrupt data
- Tools: Netflix Chaos Monkey, AWS Fault Injection Simulator
- **Interview mention:** "We'd run game days quarterly — inject failures to verify our failover, auto-scaling, and alerting actually work."

---

### 1G. Security Basics

#### 57. Authentication vs Authorization

- **Authentication (AuthN):** Who are you? (Login, JWT, OAuth)
- **Authorization (AuthZ):** What can you do? (Roles, permissions, RBAC)

**Common patterns:** JWT tokens for stateless auth, OAuth 2.0 for third-party access, API keys for service-to-service.

**OAuth 2.0 Authorization Code Flow (know this flow — asked at Google, Meta, Stripe):**
```
1. User clicks "Login with Google" → App redirects to Google's auth server
2. User logs in + consents → Google redirects back with authorization CODE
3. App's backend exchanges CODE for access_token + refresh_token (server-to-server)
4. App uses access_token to call Google APIs on user's behalf
5. When access_token expires (15-60 min), use refresh_token to get a new one
```

**JWT Structure (3 parts, base64-encoded, separated by dots):**
```
header.payload.signature

Header:  {"alg": "RS256", "typ": "JWT"}
Payload: {"user_id": "123", "role": "admin", "exp": 1711036800}
Signature: HMAC-SHA256(header + "." + payload, secret_key)
```

**Why JWT for microservices?** Stateless — each service can verify the token independently without calling an auth service. The signature proves it hasn't been tampered with.

**Token Refresh Flow:**
- Access token: short-lived (15-60 min). Sent with every API request.
- Refresh token: long-lived (7-30 days). Stored securely. Used ONLY to get new access tokens.
- Why two tokens? If access token is stolen, damage is limited to its short lifetime. Refresh token is only sent to the auth server, reducing exposure.

**When to use what:**
| Method | Use Case |
|--------|----------|
| **JWT** | Stateless auth between microservices, mobile apps |
| **OAuth 2.0** | Third-party access ("Login with Google"), API access delegation |
| **API Key** | Service-to-service auth (internal), rate limiting per client |
| **Session cookie** | Traditional web apps with server-side sessions |

---

#### 58. Encryption

- **At rest:** Data encrypted in storage (AES-256). Protects against physical theft.
- **In transit:** Data encrypted during transmission (TLS/HTTPS). Protects against eavesdropping.
- **End-to-end:** Only sender and receiver can decrypt (e.g., WhatsApp). Server cannot read content.

---

#### 59. DDoS Protection

**What:** Distributed Denial of Service — overwhelming a service with traffic.

**Mitigations:** Rate limiting, CDN (absorbs traffic), WAF (Web Application Firewall), auto-scaling, IP blacklisting.

---

### 1H. API Design Fundamentals

> This section is tested in 60%+ of SDE2 interviews. Meta's "Product Architecture" round is essentially API design. Google drills into API contracts. Amazon's LLD round often starts with "define the APIs first."

#### 60. RESTful API Design

**Resource Naming:**
```
✅ Good:
GET    /users/{id}              — Get user
GET    /users/{id}/orders       — Get user's orders
POST   /users/{id}/orders       — Create order for user
PUT    /orders/{id}             — Update entire order
PATCH  /orders/{id}             — Partial update
DELETE /orders/{id}             — Delete order

❌ Bad:
GET    /getUserOrders            — Verb in URL
POST   /createOrder              — Redundant with HTTP method
GET    /users/getAll             — "getAll" is implied by GET /users
```

**HTTP Status Codes (Know These Cold):**
| Code | Meaning | When to Use |
|------|---------|-------------|
| 200 | OK | Successful GET/PUT/PATCH |
| 201 | Created | Successful POST that creates a resource |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Invalid input / validation error |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Authenticated but not authorized |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource / version conflict |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server failure |
| 503 | Service Unavailable | Server overloaded / maintenance |

#### 61. Pagination

| Strategy | How It Works | Pros | Cons |
|----------|-------------|------|------|
| **Offset-based** | `?offset=20&limit=10` | Simple, supports "jump to page" | Slow on large datasets (DB scans), inconsistent with inserts |
| **Cursor-based** | `?cursor=eyJpZCI6MTIzfQ&limit=10` | Fast (seeks to cursor), consistent | No "jump to page N", opaque cursor |
| **Keyset** | `?after_id=123&limit=10` | Fast, simple | Requires sortable unique key |

**Interview default:** "For feeds and timelines, I'd use cursor-based pagination — it's consistent even when new items are inserted, and it's O(1) seek time with an index. The cursor is a base64-encoded `{last_seen_id, last_seen_timestamp}`."

#### 62. API Versioning

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| **URL path** | `/v1/users`, `/v2/users` | Explicit, easy to route | URL pollution |
| **Header** | `Accept: application/vnd.api+json;version=2` | Clean URLs | Hidden, harder to test |
| **Query param** | `/users?version=2` | Easy to test | Clutters params |

**Interview default:** URL path versioning (`/v1/`) — it's the most common and easiest to reason about in a design discussion.

#### 63. Error Response Design

```json
{
  "error": {
    "code": "INSUFFICIENT_BALANCE",
    "message": "Account balance is below the required amount",
    "details": {
      "required": 100.00,
      "available": 42.50,
      "currency": "USD"
    },
    "request_id": "req_abc123",
    "timestamp": "2026-03-18T10:00:00Z"
  }
}
```

**Key principles:** Machine-readable error code (not just HTTP status), human-readable message, request ID for debugging, structured details for client handling.

#### 64. Idempotency in APIs

**What:** Ensuring that making the same API call multiple times produces the same result. Critical for payment and order APIs where network retries can cause duplicates.

**Implementation:**
1. Client generates a unique `Idempotency-Key` header (UUID) per logical operation
2. Server checks if this key was already processed (lookup in Redis/DB)
3. If found → return cached response without re-executing
4. If not found → execute, store result keyed by idempotency key with TTL (24-48 hours)

**Which HTTP methods are naturally idempotent?**
- GET, PUT, DELETE → idempotent by definition
- POST → NOT idempotent (needs explicit idempotency key)

**Interview talking point:** "All our write APIs accept an `Idempotency-Key` header. The client generates a UUID per operation. On the server, we check Redis for this key — if it exists, we return the cached response. This prevents double-charges when the client retries a timed-out payment request."

---

### 1I. Cost Awareness

> Amazon interviewers especially ask "What's the cost trade-off?" This section turns "I'd use X" into "I'd use X because it costs Y and saves Z."

#### 65. Cost-Aware Design Decisions

**Compute:**
| Decision | Cheaper Option | When |
|----------|---------------|------|
| Always-on server vs serverless | Lambda/serverless | <1M requests/month, spiky traffic |
| Always-on server vs serverless | EC2/containers | Sustained high traffic (>50% utilization) |
| Reserved vs on-demand | Reserved instances | Predictable baseline load (save 40-60%) |
| Spot instances | Spot | Fault-tolerant batch jobs (save 60-90%) |

**Storage:**
| Decision | Cost Implication |
|----------|-----------------|
| S3 Standard vs S3 Infrequent Access | IA is 45% cheaper for rarely accessed data |
| S3 vs S3 Glacier | Glacier is 80% cheaper but retrieval takes minutes-hours |
| DynamoDB on-demand vs provisioned | On-demand for unpredictable traffic; provisioned + auto-scaling for steady load |
| Cache everything vs cache hot data | Cache only top 20% of data (Pareto principle) — caching cold data wastes memory |

**Architecture:**
| Decision | Cost Implication |
|----------|-----------------|
| Fan-out-on-write vs fan-out-on-read | Write is more expensive (N writes per post) but reads are free. For 99% of users, write fan-out is cheaper overall. For celebrities (>100K followers), read fan-out saves massive write costs. |
| Sync processing vs async queue | Async queues absorb spikes → fewer servers needed → lower cost |
| Multi-region active-active | 2-3x infrastructure cost. Only justify for global latency or compliance requirements. |

**Interview talking point:** "I'd use DynamoDB on-demand mode initially since traffic is unpredictable. Once we have 3 months of data, we'd switch to provisioned capacity with auto-scaling — that typically saves 40% at steady state. For image storage, frequently accessed images stay in S3 Standard, but anything not accessed in 30 days transitions to S3 IA via lifecycle policy."

---

### 1J. Concurrency & Thread Safety

> This section is tested in 30%+ of LLD rounds. When the interviewer says "make this thread-safe" or "how do you handle concurrent access?", this is what they want.

#### 66. Thread Safety Fundamentals

**What:** Ensuring that shared data is accessed correctly when multiple threads execute concurrently.

**Core primitives:**

| Primitive | What It Does | When to Use |
|-----------|-------------|-------------|
| **Mutex/Lock** | Only one thread can hold it at a time | Protecting critical sections (e.g., updating a shared counter) |
| **Read-Write Lock** | Multiple readers OR one writer | Read-heavy data (e.g., cache — many reads, rare writes) |
| **Semaphore** | Allows N concurrent accesses | Connection pools, rate limiting (e.g., max 10 DB connections) |
| **CAS (Compare-And-Swap)** | Atomic update: "set to Y only if currently X" | Lock-free counters, optimistic concurrency |

**Common thread-safe patterns:**

```java
// 1. Lock-based: Simple but can cause contention
import java.util.concurrent.locks.ReentrantLock;

public class ThreadSafeCounter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public int increment() {
        lock.lock();
        try {
            return ++count;
        } finally {
            lock.unlock();
        }
    }
}

// 2. Read-Write Lock: Better for read-heavy workloads
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteCache<K, V> {
    private final Map<K, V> data = new HashMap<>();
    private final ReadWriteLock lock = new ReentrantReadWriteLock();

    public V get(K key) {
        lock.readLock().lock();
        try {
            return data.get(key);
        } finally {
            lock.readLock().unlock();
        }
    }

    public void put(K key, V value) {
        lock.writeLock().lock();
        try {
            data.put(key, value);
        } finally {
            lock.writeLock().unlock();
        }
    }
}
```

**Producer-Consumer pattern (asked frequently):**
```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class TaskProcessor {
    private final BlockingQueue<Runnable> queue;

    public TaskProcessor(int numWorkers) {
        this.queue = new LinkedBlockingQueue<>(100); // bounded — blocks when full
        for (int i = 0; i < numWorkers; i++) {
            Thread worker = new Thread(this::workerLoop);
            worker.setDaemon(true);
            worker.start();
        }
    }

    public void submit(Runnable task) throws InterruptedException {
        queue.put(task); // blocks if queue full (backpressure)
    }

    private void workerLoop() {
        while (true) {
            try {
                Runnable task = queue.take(); // blocks if queue empty
                task.run();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
}
```

**Common interview follow-ups:**
- "Make your LRU Cache thread-safe" → Wrap `get`/`put` in a lock. For better performance, use a read-write lock (reads don't block each other).
- "Design a thread-safe singleton" → Double-checked locking or use language-level guarantees (Python module-level, Java `enum`).
- "How do you avoid deadlocks?" → Always acquire locks in the same order. Use timeouts on lock acquisition. Prefer lock-free structures when possible.

**Interview talking point:** "I'd use a read-write lock for the cache since our read:write ratio is 100:1. Multiple threads can read concurrently, and only writes acquire an exclusive lock. For the task queue, I'd use a bounded blocking queue with a thread pool — this gives us natural backpressure when the system is overloaded."

---

#### 67. Kubernetes / Container Orchestration Basics

**What:** Container orchestration platform for deploying, scaling, and managing containerized applications. At SDE2 level, know the vocabulary so you can use it naturally — don't explain K8s internals.

**Key vocabulary (translate to interview language):**
| K8s Term | What to Say in Interview |
|----------|------------------------|
| **Pod** | "An instance of our service" |
| **Deployment** | "Auto-scaling group with rolling updates" |
| **Service** | "Internal load balancer for our pods" |
| **HPA** | "Auto-scaling policy based on CPU/QPS" |

✅ **You're done when:** You can say "I'd deploy with 3 replicas and auto-scale on CPU." You do NOT need to discuss ConfigMaps, Namespaces, ingress controllers, or Helm charts in a system design interview.

---

#### 68. Service Mesh *(Know the name — one sentence max)*

**What:** Infrastructure layer that handles service-to-service communication (encryption, retries, tracing) transparently via sidecar proxies.

**SDE2 level:** One sentence when asked about inter-service security: "For service-to-service communication, we'd use a service mesh to handle encryption and retries transparently." Don't name-drop Istio/Envoy unless the interviewer asks.

---

#### 69. Capacity Planning

> 🔶 **STRETCH** — At SDE2, back-of-envelope math (concept #6) is sufficient. This section is for extra credit.

**SDE2 minimum:** "I'd plan for 2x current peak with redundancy. Auto-scaling handles daily fluctuations."

**Full framework (if asked):**
1. Measure current load: QPS, p99 latency, CPU utilization per instance
2. Target 60-70% utilization (headroom for spikes)
3. Project growth: historical rate × time horizon
4. N+2 redundancy (can lose 2 instances and still serve peak)

---

### 1K. Commonly Missed Concepts (Interviewer Red Flags)

> These are the concepts candidates forget, and interviewers notice immediately.

#### 70. Cache Stampede / Thundering Herd

**What:** When a popular cache key expires, hundreds of concurrent requests simultaneously hit the database to regenerate it, potentially crashing the DB.

**Solutions:**
- **Locking:** First request acquires a lock and rebuilds cache. Others wait or get stale data.
- **Early expiration:** Refresh cache before TTL expires (background refresh).
- **Staggered TTL:** Add random jitter to TTL so keys don't expire simultaneously.

**Interview talking point:** "For hot keys, I'd use a locking mechanism — the first request on cache miss acquires a distributed lock, rebuilds the cache, and all other requests wait briefly or serve stale data. This prevents thundering herd on our database."

---

#### 71. Microservices vs Monolith

**What:** Monolith = single deployable unit. Microservices = independently deployable services, each owning its data.

| | Monolith | Microservices |
|---|---|---|
| Deployment | All-or-nothing | Independent per service |
| Scaling | Scale entire app | Scale individual services |
| Complexity | Simple to start | Operational overhead (service mesh, tracing) |
| Data | Shared DB | Each service owns its DB |
| Best for | Small teams, early stage | Large orgs, independent team ownership |

**Interview talking point:** "I'd start with a modular monolith for a new service. As the team grows and we identify clear domain boundaries, we'd extract microservices. Premature microservices add complexity without benefit."

---

#### 72. Search System Internals (How Elasticsearch Works)

> Comes up whenever you add "search" to any design — e-commerce, social media, content platforms. Most candidates say "use Elasticsearch" but can't explain HOW it works. That's a red flag.

**Inverted Index — The Core Data Structure:**
```
Forward index (what a DB does):    Doc1 → "the quick brown fox"
                                    Doc2 → "the lazy brown dog"

Inverted index (what search does): "brown" → [Doc1, Doc2]
                                    "fox"   → [Doc1]
                                    "lazy"  → [Doc2]
                                    "quick" → [Doc1]
```
A search for "quick brown" → intersect posting lists → Doc1 matches both terms.

**Relevance Scoring — BM25 (the default in Elasticsearch):**
- Evolved from TF-IDF. Ranks documents by how relevant they are to the query.
- **Term Frequency (TF):** How often the term appears in the document (more = more relevant, with diminishing returns)
- **Inverse Document Frequency (IDF):** How rare the term is across all documents (rarer = more important. "the" is common → low IDF. "kubernetes" is rare → high IDF)
- You do NOT need to know the formula. Just explain: "BM25 ranks by term frequency in the document weighted by how rare the term is globally."

**Fuzzy Matching & Autocomplete:**
- **N-grams:** Break words into overlapping chunks. "search" → ["sea", "ear", "arc", "rch"]. Enables partial matching.
- **Edge n-grams:** For autocomplete. "search" → ["s", "se", "sea", "sear", "searc", "search"]. Index these, and a query for "sea" matches.
- **Fuzzy search:** Edit distance (Levenshtein). "serch" matches "search" with distance 1. Handles typos.

**Sharding Strategy for Search:**
- By document ID (even distribution, but queries hit all shards)
- By time range (for logs/events — recent data on fewer shards, faster queries)
- By tenant/category (for multi-tenant — each tenant's data on dedicated shards)

**Interview talking point:** "For product search, I'd use Elasticsearch with an inverted index. Products are indexed by name, description, and category. BM25 handles relevance ranking. For autocomplete, I'd use edge n-grams so typing 'lap' matches 'laptop'. The search index is kept in sync with the product DB via CDC (Debezium → Kafka → ES consumer)."

---

#### 73. Gossip Protocol *(🔶 STRETCH — just know the name)*

**What:** Peer-to-peer protocol where nodes exchange state with random peers, eventually converging. Used in Cassandra for failure detection. One sentence: "Cassandra uses gossip protocol for node failure detection."

---

#### 74. Write-Ahead Log (WAL)

**What:** Before any data modification, the change is first written to an append-only log. If the system crashes, replay the log to recover. Used in PostgreSQL, MySQL, Kafka.

**SDE2 level:** One sentence when discussing durability: "The database uses a write-ahead log for crash recovery."

---

#### 75. Quorum Reads/Writes

**What:** In a replicated system with N replicas, a quorum requires agreement from a majority.

**SDE2 level:** Know the rule: **W + R > N = strong consistency.** Common config: N=3, W=2, R=2.

**Interview talking point:** "With 3 replicas, I'd set W=2, R=2 for strong consistency while tolerating one node failure."

> 🔶 **STRETCH:** Detailed tuning of W/R values for different consistency levels is SDE3. At SDE2, the one-liner above is sufficient.

---

#### 76. Common Follow-Up Questions (Prepare for These)

Interviewers ALWAYS ask these after your initial design. Have answers ready:

| Follow-Up | What They're Testing | How to Answer |
|-----------|---------------------|---------------|
| "What if traffic grows 10x?" | Scaling thinking | "We'd add more app server instances, shard the DB, and increase cache capacity. The stateless design makes horizontal scaling straightforward." |
| "What if this component fails?" | Fault tolerance | "We have replicas. If the primary DB fails, the read replica is promoted. If a cache node dies, consistent hashing redistributes only 1/N of keys." |
| "How would you monitor this?" | Operational maturity | "I'd track p99 latency, error rate, QPS, and DB connection pool utilization. Alerts fire if p99 > 500ms or error rate > 1%." |
| "What are the bottlenecks?" | Self-awareness | "The write path to the database is the bottleneck at scale. I'd address it with write-behind caching and sharding." |
| "How do you handle data consistency?" | Distributed systems depth | "For this use case, eventual consistency is acceptable. We use async replication with a ~200ms lag. For the payment flow, we use synchronous replication." |
| "What would you change for a global deployment?" | Multi-region thinking | "I'd deploy in 3 regions with active-active setup, use a global load balancer for geo-routing, CDN for static content, and CRDTs or LWW for conflict resolution." |

---

## Part 2: HLD Problems — "Design X"

> For each problem: key requirements, core components, critical trade-offs, and links to the best solution resources.

---

### Tier 1: Must-Do (Top 10 — Asked Everywhere)

> **Difficulty guide for SDE1s:** Start with Easy, then Medium, then Hard. Don't jump to Hard before you can do Easy problems in 40 minutes.
>
> | Difficulty | Problems | Why |
> |-----------|----------|-----|
> | 🟢 Easy | URL Shortener, Rate Limiter, Notification System | Few components, clear trade-offs |
> | 🟡 Medium | Chat System, News Feed, YouTube, Autocomplete | Multiple deep-dive areas, fan-out decisions |
> | 🔴 Hard | Uber, Dropbox, Web Crawler | Geo-spatial, sync protocols, distributed crawling |

---

### ⭐ Fully Worked Example: URL Shortener (End-to-End Using the 4-Step Framework)

> This is exactly how you should present ANY HLD problem. Study this flow, then apply it to every other problem.

#### 🔴 Bad vs 🟢 Good Answer (Read This First)

> This is what separates No Hire from Strong Hire. Same problem, same 45 minutes, completely different outcomes.

**🔴 Bad Answer (No Hire):**
```
Interviewer: "Design a URL shortener."

Candidate: "OK so we need a database to store URLs. I'd use MySQL.
We generate a short code using MD5 hash. The user sends a long URL,
we hash it, store it, return the short code. For reads, we look up
the short code in the database and redirect.

We can add Redis for caching. And a load balancer in front.
We should use Kafka for... something. And Elasticsearch for search.

[Draws boxes: Client → LB → Server → DB, with Redis and Kafka floating]

That's basically it. Any questions?"
```

**Why this fails:**
- ❌ No requirements gathering (jumped straight to design)
- ❌ No back-of-envelope math (no idea of scale)
- ❌ No trade-off discussion ("I'd use MySQL" — why not DynamoDB? why not PostgreSQL?)
- ❌ Name-dropped Kafka and Elasticsearch with no justification
- ❌ No deep dive on the core problem (ID generation)
- ❌ No failure modes, no monitoring, no wrap-up
- ❌ Passive — waited for interviewer to ask questions

**🟢 Good Answer (Strong Hire):**
```
Interviewer: "Design a URL shortener."

Candidate: "Before I design, let me clarify requirements.

Functional: Generate short URL from long URL, redirect short → long.
Should we support custom aliases? Analytics?
Interviewer: "Custom aliases yes, analytics is optional."

Non-functional: This is read-heavy — I'd estimate 100:1 read-write.
Let me do quick math: 100M new URLs/day = ~1200 writes/sec,
120K reads/sec at peak. Each record ~500 bytes, so ~50GB/day storage.
7-char base62 gives us 3.5 trillion combinations — more than enough.

[Draws read path and write path separately]

For ID generation, I considered three approaches:
1. Auto-increment + base62 — simple but predictable (security risk)
2. MD5 hash truncated — works but needs collision handling on hot path
3. Pre-generated ID pool — moves complexity to background job

I'd go with option 3 because it keeps the write path simple and
collision-free. A background worker pre-generates batches of unique
codes into an 'available_codes' table.

For the read path, I'd use cache-aside with Redis. With 80% of
traffic hitting 20% of URLs (Pareto), I expect ~80% cache hit rate.
TTL of 24 hours for hot URLs.

The main bottleneck at scale is the write path — I'd shard the DB
by short_code using consistent hashing.

I'd monitor: redirect latency p99, cache hit ratio, error rate,
and ID pool depletion rate.

Want me to dive deeper into any of these areas?"
```

**Why this works:**
- ✅ Requirements first (functional + non-functional)
- ✅ Back-of-envelope math (QPS, storage, code space)
- ✅ Trade-off discussion ("I considered X, Y, Z — chose Z because...")
- ✅ Deep dive on core problem (ID generation with 3 alternatives)
- ✅ Failure modes and monitoring mentioned proactively
- ✅ Drove the conversation, then asked interviewer where to go deeper

**Step 1: REQUIREMENTS (5 min)**

*You say:* "Let me clarify the requirements before designing."

**Functional:**
- Given a long URL, generate a short URL (e.g., `short.ly/abc123`)
- Redirect short URL → original long URL
- Optional: custom aliases, expiration, analytics (click count)

**Non-functional:**
- High availability (reads must never fail)
- Low latency redirects (<50ms)
- Short URLs should not be guessable (no sequential IDs)

**Back-of-envelope:**
- 100M new URLs/day → ~1,200 writes/sec
- Read:write ratio ~100:1 → 120K reads/sec
- Each URL record ~500 bytes → 100M × 500B = 50GB/day → ~18TB/year
- Short code length: 7 chars of base62 = 62^7 = 3.5 trillion combinations (sufficient)

**Step 2: HIGH-LEVEL DESIGN (10 min)**

*You draw:*
```
Client → API Gateway (rate limiting, auth)
              ↓
         App Servers (stateless, auto-scaled)
              ↓                    ↓
     Write Path:              Read Path:
     POST /urls               GET /{shortCode}
         ↓                        ↓
     ID Generator          Redis Cache (check first)
         ↓                   ↓ miss      ↓ hit
     DB (write)          DB (read)    → 301 Redirect
         ↓                   ↓
     Return shortCode    Cache result → 301 Redirect
```

**APIs:**
```
POST /api/v1/urls
  Body: { "long_url": "https://...", "custom_alias": "optional", "expiry": "optional" }
  Response: { "short_url": "https://short.ly/abc123" }

GET /{shortCode}
  Response: 301 Redirect to long_url
  (301 = permanent redirect, browser caches it. Use 302 if you need analytics on every click.)
```

**Data Model:**
```
urls table:
  short_code  VARCHAR(7)  PRIMARY KEY
  long_url    TEXT        NOT NULL
  user_id     BIGINT      (nullable)
  created_at  TIMESTAMP
  expires_at  TIMESTAMP   (nullable)
  click_count BIGINT      DEFAULT 0
```

**Step 3: DEEP DIVE (20 min)**

*You say:* "I'd like to dive into ID generation and the caching strategy. Would you like me to focus on those, or is there another area?"

**ID Generation (the core problem):**

| Approach | How | Pros | Cons |
|----------|-----|------|------|
| Auto-increment + Base62 | DB auto-increment → encode to base62 | Simple, no collisions | Predictable, single DB bottleneck |
| MD5/SHA hash + truncate | Hash long URL → take first 7 chars | Deterministic | Collisions possible, need collision handling |
| Pre-generated ID pool | Background worker pre-generates IDs → stores in Redis/DB | Fast, no collision at write time | Complexity of ID pool management |
| Snowflake-like | Timestamp + machine ID + sequence | Distributed, no coordination | 64-bit → base62 = 11 chars (longer) |

*You say:* "I'd go with a pre-generated ID pool. A background service generates batches of unique 7-char base62 codes and stores them in a 'available_codes' table. When a new URL is created, we pop a code from the pool. This avoids collision handling and removes the ID generation from the hot path."

> **🧠 How I arrived at this:** I eliminated auto-increment (predictable URLs = security risk, single DB bottleneck). Hash+truncate works but needs collision handling on every write — that's complexity on the hot path. Snowflake gives 11-char codes (too long). Pre-generated pool moves complexity to a background job, keeps the write path simple. I picked the option that minimizes hot-path complexity.

**Caching:**
- Cache-aside pattern with Redis
- Key: `shortCode`, Value: `longUrl`
- TTL: 24 hours (hot URLs stay cached)
- Cache hit ratio expected: ~80% (Pareto — 20% of URLs get 80% of traffic)
- On cache miss: read from DB, populate cache, return

**Handling collisions (if using hash approach):**
- Hash long URL → check if short code exists in DB
- If collision → append a counter and rehash, or try next hash
- Bloom filter in front of DB to reduce unnecessary lookups

**Step 4: WRAP UP (5 min)**

*You say:*
- "The bottleneck is the write path at extreme scale — I'd shard the DB by short_code hash."
- "For monitoring, I'd track: redirect latency p99, cache hit ratio, error rate, and DB connection pool utilization."
- "Future extensions: analytics dashboard (click count by country/device), link expiration worker, abuse detection."

---

#### HLD-1. Design a URL Shortener (TinyURL) 🟢

**Requirements:** Generate short URL from long URL, redirect short → long, analytics (optional), custom aliases (optional).

**Key decisions:**
- **ID generation:** Base62 encoding of auto-increment ID, or MD5/SHA hash truncated to 7 chars
- **Storage:** Key-value store (DynamoDB) — key: short_code, value: long_url
- **Read:Write ratio:** ~100:1 → cache heavily
- **Cache:** Redis for hot URLs
- **Scale:** Consistent hashing for DB sharding by short_code

**Back-of-envelope:** 100M URLs/day = ~1200 writes/sec. 10B redirects/day = ~120K reads/sec.

❌ **Common mistakes:**
- Using 302 (temporary) instead of 301 (permanent) redirect — 301 lets browsers cache, reducing server load. Use 302 only if you need to track every click for analytics.
- Sequential auto-increment IDs — predictable URLs are a security risk (users can enumerate all URLs)
- Forgetting custom alias collision handling — what if two users want the same alias?
- Not mentioning abuse prevention — spam URLs, phishing links need URL validation

📺 ⭐ [ByteByteGo — URL Shortener](https://www.youtube.com/watch?v=fMZMm_0ZhK4) *(watch this first)*
📺 [Gaurav Sen — URL Shortener](https://www.youtube.com/watch?v=JQDHz72OA3c)
📖 [HelloInterview — URL Shortener](https://www.hellointerview.com/learn/system-design/problem-breakdowns/tinyurl)
📖 [SystemDesignSchool — URL Shortener](https://systemdesignschool.io/problems/url-shortener/solution)

---

#### HLD-2. Design a Rate Limiter 🟢

**Requirements:** Limit requests per client per time window. Distributed (works across multiple servers). Return 429 on exceed.

**Key decisions:**
- **Algorithm:** Token bucket (allows bursts) or sliding window counter (smooth)
- **Storage:** Redis (INCR + EXPIRE for counters)
- **Placement:** API Gateway or middleware
- **Rules:** Configurable per endpoint, per user, per IP

📺 [ByteByteGo — Rate Limiter](https://www.youtube.com/watch?v=FU4WlwfS3G0)
📖 [HelloInterview — Rate Limiter](https://www.hellointerview.com/learn/system-design/problem-breakdowns/rate-limiter)

---

#### HLD-3. Design a Chat System (WhatsApp) 🟡

**Requirements:** 1:1 messaging, group chat (up to 100), online/offline status, message delivery/read receipts, offline message storage.

**Key decisions:**
- **Real-time:** WebSocket connections for online users
- **Message flow:** Sender → Chat Server → check if recipient online → deliver via WebSocket OR store for later
- **Storage:** Cassandra/DynamoDB for messages (partition by chat_id, sort by timestamp)
- **Offline delivery:** Store undelivered messages, push notification via APNS/FCM
- **Group chat:** Fan-out message to all group members
- **Ordering:** Sequence IDs per conversation

❌ **Common mistakes:**
- Using HTTP polling instead of WebSockets — massive overhead at scale
- Forgetting offline message delivery — what happens when user comes back online?
- Not separating connection servers from app servers (see concept #30)
- Ignoring message ordering — network delays can deliver messages out of order

📺 ⭐ [Gaurav Sen — Design WhatsApp](https://www.youtube.com/watch?v=vvhC64hQZMk) *(watch this first)*
📖 [HelloInterview — WhatsApp](https://www.hellointerview.com/learn/system-design/problem-breakdowns/whatsapp)
📖 [ByteByteGo — Chat System](https://bytebytego.com/courses/system-design-interview/design-a-chat-system)

---

#### HLD-4. Design a News Feed (Twitter/Facebook) 🟡

**Requirements:** Users follow others, post content, see personalized feed of followed users' posts.

**Key decisions:**
- **Fan-out on write (push):** When user posts, push to all followers' feed cache. Fast reads, expensive writes for celebrities.
- **Fan-out on read (pull):** Compute feed on request by merging recent posts from followed users. Slow reads, cheap writes.
- **Hybrid:** Push for normal users, pull for celebrities (>100K followers).
- **Feed storage:** Pre-computed feed in Redis (sorted set by timestamp).
- **Ranking:** Chronological (simple) or ML-ranked (complex).

📺 ⭐ [Gaurav Sen — Design Twitter](https://www.youtube.com/watch?v=wYk0xPP_P_8) *(watch this first)*
📺 [ByteByteGo — News Feed](https://www.youtube.com/watch?v=R_agd5qjc1I)

❌ **Common mistakes:**
- Using only fan-out-on-write — explodes for celebrities with 10M+ followers
- Forgetting the hybrid approach — push for normal users, pull for celebrities
- Not mentioning feed ranking — chronological is fine for MVP, but interviewers expect you to mention ML ranking as an extension

---

#### HLD-5. Design a Video Streaming Platform (YouTube/Netflix) 🟡

**Requirements:** Upload videos, transcode to multiple resolutions, stream globally with low latency, search, recommendations.

**Key decisions:**
- **Upload:** Chunked upload → message queue → transcoding workers (FFmpeg) → multiple resolutions (360p, 720p, 1080p, 4K)
- **Storage:** Blob storage (S3) for video files, relational DB for metadata
- **Delivery:** CDN for global distribution, adaptive bitrate streaming (HLS/DASH)
- **Streaming protocol:** HLS (HTTP Live Streaming) — video split into small segments, client requests segments sequentially

❌ **Common mistakes:**
- Storing video files in a database instead of blob storage
- Forgetting transcoding is async — user uploads, gets a "processing" status, notified when ready
- Not mentioning adaptive bitrate — client switches quality based on bandwidth

📺 ⭐ [ByteByteGo — YouTube Design](https://www.youtube.com/watch?v=jPKTo1iGQiE) *(watch this first)*
📖 [ByteByteGo — YouTube Written](https://bytebytego.com/courses/system-design-interview/design-youtube)

---

#### HLD-6. Design a File Storage Service (Dropbox/Google Drive) 🔴

**Requirements:** Upload/download files, sync across devices, notifications on changes, versioning (optional).

**Key decisions:**
- **Chunking:** Split large files into 4MB chunks. Only upload changed chunks (deduplication).
- **Storage:** S3 for file chunks, relational DB for metadata (file tree, versions, permissions)
- **Sync:** Client watches local filesystem → detects changes → uploads changed chunks → notifies other clients via WebSocket/long polling
- **Conflict resolution:** If two clients edit same file, save both versions and let user resolve

❌ **Common mistakes:**
- Uploading entire files instead of chunks — wastes bandwidth for small edits to large files
- Forgetting conflict resolution — two users edit the same file offline, both come online

📺 ⭐ [Gaurav Sen — Design Dropbox](https://www.youtube.com/watch?v=U0xTu6E2CT8) *(watch this first)*

---

#### HLD-7. Design a Ride-Hailing Service (Uber) 🔴

**Requirements:** Rider requests ride, match with nearest available driver, real-time tracking, fare calculation, payments.

**Key decisions:**
- **Location tracking:** Drivers send GPS pings every 3-5 seconds → stored in-memory (geospatial index)
- **Geospatial indexing:** Geohash or Quadtree to find nearby drivers efficiently
- **Matching:** Find available drivers within radius → rank by distance/ETA → send request → timeout → try next
- **Real-time updates:** WebSocket for driver location streaming to rider
- **Partitioning:** By city/region for data locality

❌ **Common mistakes:**
- Using a regular DB query for "find nearby drivers" — need geospatial index (Geohash/Quadtree)
- Forgetting driver location is ephemeral — store in-memory (Redis), not in persistent DB
- Not handling driver rejection/timeout — need a matching queue with fallback to next driver

📺 ⭐ [Gaurav Sen — Design Uber](https://www.youtube.com/watch?v=umWABit-wbk) *(watch this first)*
📖 [HelloInterview — Uber](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber)

---

#### HLD-8. Design a Notification System 🟢

**Requirements:** Send push notifications, SMS, and email. Support millions of users. Prioritization, rate limiting, user preferences.

**Key decisions:**
- **Architecture:** Notification service → message queue (per channel) → channel-specific workers (push/SMS/email)
- **Push:** APNS (iOS), FCM (Android)
- **Deduplication:** Idempotency key to prevent duplicate notifications
- **User preferences:** DB table for opt-in/opt-out per channel per notification type
- **Priority queue:** Urgent notifications (OTP) get higher priority than marketing

❌ **Common mistakes:**
- Single queue for all channels — SMS, email, push have different latency requirements. Use separate queues.
- Forgetting user preferences — users must be able to opt out per notification type
- Not mentioning rate limiting — don't spam users with 50 notifications in a minute

📺 ⭐ [ByteByteGo — Notification System](https://www.youtube.com/watch?v=bBTPZ9NdSk8) *(watch this first)*

---

#### HLD-9. Design a Web Crawler 🔴

**Requirements:** Crawl billions of web pages, extract content, handle politeness (robots.txt), avoid duplicates, handle traps.

**Key decisions:**
- **Architecture:** URL frontier (priority queue) → fetcher workers → content parser → URL extractor → dedup (Bloom filter) → back to frontier
- **Politeness:** Respect robots.txt, rate limit per domain
- **Dedup:** Bloom filter for URL dedup, content hash (SimHash) for near-duplicate detection
- **Storage:** Blob storage for raw HTML, search index for parsed content
- **Scale:** Distributed workers, partition URL frontier by domain

❌ **Common mistakes:**
- Forgetting politeness — crawling a site too aggressively gets you blocked
- No URL dedup — infinite loops on circular links
- Not handling spider traps — dynamically generated infinite URLs (e.g., calendar pages)

📺 ⭐ [ByteByteGo — Web Crawler](https://www.youtube.com/watch?v=BKZxZwUgL3Y) *(watch this first)*

---

#### HLD-10. Design Search Autocomplete / Typeahead 🟡

**Requirements:** As user types, suggest top 5-10 completions. Low latency (<100ms). Based on popularity/frequency.

**Key decisions:**
- **Data structure:** Trie (prefix tree) with frequency counts at each node
- **Storage:** In-memory trie on each server, rebuilt periodically from analytics data
- **Ranking:** Top-K by frequency at each prefix node (pre-computed)
- **Updates:** Offline pipeline aggregates search logs → rebuilds trie → deploys to servers
- **Optimization:** Only query after 2-3 characters typed, debounce 200ms

❌ **Common mistakes:**
- Querying on every keystroke — use debouncing (200ms) and minimum 2-3 chars
- Real-time trie updates — too expensive. Use offline batch rebuild.
- Forgetting personalization — mention as extension: "blend global popularity with user's search history"

📺 ⭐ [ByteByteGo — Autocomplete](https://www.youtube.com/watch?v=us0qySiUsGU) *(watch this first)*

---

### Tier 2: High Frequency (Next 10)

#### HLD-11. Design Instagram

Photo upload, feed, follow, explore. Combines file storage (S3 + CDN) with news feed design.

📖 [IGotAnOffer — Instagram Design (with diagram)](https://igotanoffer.com/blogs/tech/system-design-interviews#instagram)

#### HLD-12. Design an E-Commerce Platform (Amazon)

Product catalog, search, cart, orders, payments, inventory. Key: ACID for orders, eventual consistency for catalog.

📖 [DesignGurus — E-Commerce](https://www.designgurus.io/course-play/mastering-system-design-interviews-a-crash-course/doc/designing-an-ecommerce-system)

#### HLD-13. Design a Distributed Cache (Redis)

Consistent hashing, eviction policies, replication, persistence. Key: cache-aside pattern, handling cache stampede.

📺 [Gaurav Sen — Distributed Cache](https://www.youtube.com/watch?v=iuqZvajTOyA)

#### HLD-14. Design a Key-Value Store (DynamoDB)

Consistent hashing, replication, conflict resolution (vector clocks), gossip protocol for failure detection.

📺 [ByteByteGo — Key-Value Store](https://www.youtube.com/watch?v=rnZmdmlR-2M)

#### HLD-15. Design Google Maps / Proximity Service

Geospatial indexing (Quadtree/Geohash), routing algorithms (Dijkstra/A*), tile-based map rendering, ETA calculation.

📺 [codeKarle — Google Maps](https://www.youtube.com/watch?v=jk3yvVfNvds)

#### HLD-16. Design a Ticket Booking System (BookMyShow)

Seat selection with distributed locking, payment with timeout, handling concurrent bookings. Key: pessimistic locking or optimistic concurrency.

📺 [sudoCODE — BookMyShow](https://www.youtube.com/watch?v=lBAwJgoO3Ek)

#### HLD-17. Design a Music Streaming Platform (Spotify)

Audio storage (blob), metadata DB, CDN for streaming, offline caching, search, playlists. Similar to video streaming but simpler.

📖 [IGotAnOffer — Spotify Design](https://igotanoffer.com/blogs/tech/system-design-interviews#streaming)

#### HLD-18. Design a Food Delivery System (DoorDash/Swiggy)

Combines Uber (driver matching, real-time tracking) + E-commerce (orders, payments) + restaurant management.

📺 [codeKarle — Food Delivery](https://www.youtube.com/watch?v=kvYjOI1JBb0)

#### HLD-19. Design a Real-Time Stock Ticker

WebSocket for live prices, time-series DB for historical data, pub/sub for price updates, candlestick aggregation.

📺 [sudoCODE — Stock Exchange](https://www.youtube.com/watch?v=dUMWMZmMsVE)

#### HLD-20. Design a Distributed Message Queue (Kafka)

Topics, partitions, consumer groups, offset management, replication, ordering guarantees. Key: partition-level ordering, at-least-once delivery.

📺 [ByteByteGo — Kafka Architecture](https://www.youtube.com/watch?v=UNUz1-msbOM)

---

### Tier 3: Moderate Frequency

> 🔶 **SDE2 guidance:** From this tier, prep **at most 2-3 problems** based on your target company. The rest are SDE3/Staff-level complexity. If you're short on time, skip this entire tier — Tier 1 + Tier 2 covers 90% of SDE2 interviews. (Next 10)

#### HLD-21. Design a Collaborative Document Editor (Google Docs)

> 🔶 **STRETCH** — OT/CRDTs are SDE3 depth. At SDE2, know: "Real-time sync via WebSockets, operational transformation for conflict resolution." Don't prep OT internals.

Operational Transformation (OT) or CRDTs for conflict-free concurrent editing. WebSocket for real-time sync.

📺 [Gaurav Sen — Google Docs](https://www.youtube.com/watch?v=2auwirNBvGg)

#### HLD-22. Design a Payment System

Idempotency, double-entry bookkeeping, payment state machine, retry with reconciliation, PCI compliance.

📺 [ByteByteGo — Payment System](https://www.youtube.com/watch?v=olfaBgJrUBI)

#### HLD-23. Design a Hotel Booking System

Search with date ranges, inventory management, distributed locking for reservations, overbooking strategy.

#### HLD-24. Design a CDN *(Rarely asked standalone — CDN is a component you mention, not a full design question)*

Edge servers, cache invalidation, origin pull vs push, DNS-based routing, cache hierarchy. Know this as a building block, not a standalone problem.

#### HLD-25. Design a Metrics/Monitoring System (Datadog)

Time-series data ingestion, aggregation pipeline, dashboarding, alerting rules engine.

📺 [ByteByteGo — Metrics Monitoring](https://www.youtube.com/watch?v=SsEYNqkJbCE)

#### HLD-26. Design a Distributed Job Scheduler

> 🔶 **STRETCH** — Rarely asked at SDE2. More common at SDE3/Staff.

Job queue, worker pool, cron-like scheduling, retry/dead-letter, exactly-once execution, sharded job assignment.

#### HLD-27. Design an Ad Click Aggregation System

Real-time event streaming (Kafka), MapReduce aggregation, deduplication, time-window counting.

📺 [ByteByteGo — Ad Click Aggregation](https://www.youtube.com/watch?v=6GXLP-VRXnI)

#### HLD-28. Design a Social Graph + Friend Recommendations

Graph database or adjacency list, BFS for friend-of-friend, collaborative filtering for recommendations.

#### HLD-29. Design a Global Rate Limiter

> 🔶 **STRETCH** — This is explicitly an SDE3 prompt per systemdesignhandbook.com ("multi-tenant rate-limiting platform for all services in your org, used by over 80 teams"). At SDE2, you design a rate limiter for a single service (HLD-2).

Distributed token bucket across data centers, eventual consistency of counters, Redis Cluster.

#### HLD-30. Design a Search Engine (Simplified Google)

Web crawler → indexer (inverted index) → query processor → ranker (TF-IDF, PageRank). Sharded index.

📺 [Gaurav Sen — Search Engine](https://www.youtube.com/watch?v=CeGtqYdA-w4)

---

#### HLD-31. Design a Data Pipeline / Log Aggregation System

**Requirements:** Ingest logs/events from thousands of services, process and store for querying, support dashboards and alerting.

**Key components:**
```
Services → Log Agents → Kafka (ingestion buffer)
                              ↓
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
      Stream Consumers   Raw Storage     Batch Analytics
      (real-time alerts) (S3)           (data warehouse)
              ↓               ↓               ↓
         Dashboards      Ad-hoc Queries  Historical Reports
```

**Key decisions:**
- **Ingestion:** Kafka as the central buffer — decouples producers from consumers, handles spikes, provides replay
- **Real-time path:** Kafka consumers process events for alerting (error rate spikes, latency breaches)
- **Batch path:** Separate consumer writes to S3 for historical analytics and ad-hoc queries
- **Partitioning:** Partition Kafka topics by service_name or timestamp for parallelism
- **Retention:** Hot data (7 days) in Kafka, warm data (90 days) in S3 Standard, cold data (1+ year) in S3 Glacier

**Interview talking point:** "Log agents on each service push structured events to Kafka topics partitioned by service name. Stream consumers process events for real-time alerting, while a separate consumer writes to S3 for batch analytics. This gives us both real-time and historical querying."

📺 [ByteByteGo — Log Aggregation](https://www.youtube.com/watch?v=rnZmdmlR-2M)

---

#### HLD-32. Design a Feature Flags / A/B Testing Platform

**Requirements:** Toggle features on/off without deployment, target specific user segments, run A/B experiments, track metrics per variant.

**Key components:**
```
SDK (client-side) → Feature Flag Service → Flag Store (Redis + DB)
                          ↓
                    Targeting Rules Engine
                    (user_id, %, geo, attributes)
                          ↓
                    Event Collector → Analytics Pipeline
                          ↓
                    Experiment Dashboard (conversion rates per variant)
```

**Key decisions:**
- **Evaluation:** Client-side (SDK caches flags, evaluates locally — fast, works offline) vs server-side (API call per evaluation — always fresh, more control). Hybrid: SDK fetches flag config periodically, evaluates locally.
- **Consistent assignment:** Hash `user_id + experiment_id` to deterministically assign users to variants. Same user always sees same variant.
- **Gradual rollout:** Start at 1% → 5% → 25% → 100%. If error rate spikes, auto-rollback.
- **Metrics tracking:** Track conversion metrics per variant. Let the data team determine when results are significant — that's not the SDE2's job in this design.
- **Flag lifecycle:** Active → Completed → Archived. Stale flags (>90 days) should be cleaned up to reduce tech debt.

**Interview talking point:** "The SDK fetches flag configurations every 60 seconds and caches locally. For each flag evaluation, it hashes `user_id + flag_key` to get a consistent percentage bucket — so user assignment is deterministic without server calls. Events (flag evaluated, conversion happened) are batched and sent to Kafka for the analytics pipeline."

---

### Tier 4: Emerging / AI-Era (5 Problems)

> 🔶 **SDE2 guidance:** These are only asked at **Anthropic, OpenAI, and AI-focused teams**. For general FAANG SDE2, skip this entire tier. If you're targeting AI companies, prep only HLD-33 (RAG Pipeline) — it's the "Design a News Feed" of AI interviews. The rest are SDE3/Staff.

#### HLD-33. Design an LLM-Powered Search System (RAG Pipeline)

**Requirements:** User asks a natural language question → system retrieves relevant documents from a private knowledge base → LLM generates an answer grounded in those documents (not hallucinated).

**Key components:**
```
User Query → Embedding Model → Vector DB (similarity search)
                                      ↓
                              Top-K relevant chunks
                                      ↓
                    Prompt = "Given these documents: {chunks}, answer: {query}"
                                      ↓
                              LLM generates response
```

**Key decisions:**
- **Chunking strategy:** Split documents into 500-1000 token chunks with 50-100 token overlap. Too small = lost context. Too large = diluted relevance.
- **Embedding model:** OpenAI text-embedding-3-large, Cohere embed-v3, or open-source (e5-large). Dimensions: 768-3072.
- **Vector database:** Pinecone (managed), Weaviate (open-source), Milvus, Qdrant, pgvector (PostgreSQL extension). Index type: HNSW for fast approximate nearest neighbor search.
- **Retrieval:** Top-K (typically K=5-10) by cosine similarity. Hybrid search = vector similarity + BM25 keyword search for better recall.
- **Reranking:** After initial retrieval, use a cross-encoder reranker to re-score and reorder results for higher precision.
- **Caching:** Cache embeddings for repeated queries. Cache LLM responses for identical query+context pairs.

**Scale considerations:**
- Embedding generation: batch process documents offline, store in vector DB
- Query path: embed query (50ms) → vector search (10-50ms) → LLM generation (500-2000ms)
- Bottleneck is LLM inference — use streaming responses for perceived latency

**Interview talking point:** "I'd build a RAG pipeline: documents are chunked, embedded, and stored in a vector database. At query time, we embed the user's question, retrieve the top 5 most similar chunks via HNSW search, then pass them as context to the LLM. I'd use hybrid search (vector + BM25) for better recall, and a cross-encoder reranker for precision."

#### HLD-34. Design a Batched Inference System

**Requirements:** Serve ML model predictions at scale. Optimize for throughput (batch) vs latency (real-time).

**Key decisions:**
- **Real-time path:** Request → API Gateway → Model Server (GPU) → Response. Latency: 50-200ms.
- **Batch path:** Requests accumulate in queue → Batch Accumulator (waits for N requests or T milliseconds) → GPU inference on batch → Route responses back to callers.
- **Why batch?** GPUs are most efficient processing many inputs simultaneously. A batch of 32 inputs may only take 1.5x the time of a single input.
- **Auto-scaling:** Scale GPU instances based on queue depth, not CPU.

#### HLD-35. Design a Real-Time Analytics Pipeline
Kafka → stream consumers → aggregation → time-series DB → dashboard.

#### HLD-36. Design a Webhook Delivery System
Event queue → delivery workers → retry with exponential backoff → dead letter queue → delivery status tracking.

#### HLD-37. Design an In-Memory Database (Redis-like)
Hash table in memory, persistence (RDB snapshots, AOF log), replication, eviction policies, data structures.

---

## Part 3: LLD Problems — Object-Oriented Design (25 Problems)

> For each: key classes, patterns used, and resource links. Practice modeling these with actual code.

**General approach for every LLD problem:**
1. Clarify requirements and use cases
2. Identify core entities (nouns → classes)
3. Identify actions (verbs → methods)
4. Define relationships (has-a, is-a, uses)
5. Apply relevant design patterns
6. Define APIs / method signatures
7. Handle edge cases

### 🧠 The 5-Minute LLD Derivation Method (For Problems You've Never Seen)

> This is more valuable than any single worked example. It transfers to ANY novel problem.

```
Step 1: NOUNS → CLASSES
  Read the problem. Underline every noun. These are your candidate classes.
  "Design a parking lot" → ParkingLot, Floor, Spot, Vehicle, Ticket, Payment

Step 2: VERBS → METHODS
  Underline every verb/action. These become methods on your classes.
  "park a vehicle" → ParkingLot.parkVehicle()
  "calculate fee" → Payment.calculateFee()
  "find available spot" → ParkingLot.findSpot()

Step 3: "WHO OWNS WHAT?" → RELATIONSHIPS
  Ask: Does A contain B? (composition) Does A use B? (association)
  ParkingLot contains Floors. Floor contains Spots. Spot holds a Vehicle.

Step 4: "WHAT VARIES?" → STRATEGY PATTERN
  Ask: What behavior could change or have multiple implementations?
  Pricing varies (hourly, flat, weekend) → PricingStrategy interface
  Vehicle type varies (car, truck, motorcycle) → but inheritance is fine here

Step 5: "WHAT CHANGES STATE?" → STATE PATTERN
  Ask: Does any entity go through distinct states with different behavior?
  Elevator: Idle → Moving → DoorOpen → Idle
  Vending Machine: NoCoin → HasCoin → Dispensing
  Order: Placed → Confirmed → Shipped → Delivered
```

**Practice this method, not the solutions.** If you can derive classes from nouns and patterns from "what varies?", you can handle any LLD problem — even ones not in this guide.

📖 [Master Reference — Grokking OOD (GitHub)](https://github.com/tssovi/grokking-the-object-oriented-design-interview)
📖 [AlgoMaster — LLD Problems](https://algomaster.io/learn/lld)
📖 [Medium — Top 10 LLD Problems Solved](https://medium.com/@prashant558908/solving-top-10-low-level-design-lld-interview-questions-in-2024-302b6177c869)

---

### UML Basics for Interviews

> You don't need to be a UML expert, but you need to draw class diagrams that interviewers can read. Here's the minimum you need.

**Class box:**
```
┌──────────────────────┐
│     ClassName         │  ← Name (bold/centered)
├──────────────────────┤
│ - privateField: Type  │  ← Attributes (- private, + public, # protected)
│ + publicField: Type   │
├──────────────────────┤
│ + methodName(): Type  │  ← Methods
│ - helperMethod(): void│
└──────────────────────┘
```

**Relationship arrows (memorize these 4):**

| Arrow | Meaning | Example | How to Read |
|-------|---------|---------|-------------|
| `A ──▶ B` (solid + filled arrow) | **Association** | `Driver ──▶ Car` | "Driver has/uses a Car" |
| `A ◇──▶ B` (diamond + arrow) | **Aggregation** | `Team ◇──▶ Player` | "Team has Players" (Players exist independently) |
| `A ◆──▶ B` (filled diamond) | **Composition** | `House ◆──▶ Room` | "House owns Rooms" (Rooms die with House) |
| `A ──▷ B` (open triangle) | **Inheritance** | `Car ──▷ Vehicle` | "Car is-a Vehicle" |
| `A ┈┈▷ B` (dashed + triangle) | **Implements** | `Dog ┈┈▷ Animal` | "Dog implements Animal interface" |

**Multiplicity:** `1`, `0..1`, `1..*`, `0..*`, `*` — write near the arrow to show cardinality.

**Interview tip:** You don't need perfect UML. A clean box-and-arrow diagram with class names, key attributes, and relationship labels is sufficient. Interviewers care about your modeling decisions, not UML syntax.

---

### Tier 1: Most Frequently Asked

---

### ⭐ Fully Worked: Parking Lot (End-to-End LLD)

> This is the gold standard LLD problem. Study this structure, then apply it to every other LLD problem.

**Step 1: Requirements**
- Multi-floor parking lot with different spot sizes (small, medium, large)
- Vehicles: motorcycle, car, truck
- Entry/exit panels issue tickets and process payments
- Pricing: hourly rate varies by spot size
- Track availability per floor

**Step 2: Core Classes & Relationships**

```
┌─────────────┐       ┌──────────────┐
│ ParkingLot   │──────▶│ ParkingFloor  │
│ (Singleton)  │ 1..N  │              │
└─────────────┘       └──────┬───────┘
                              │ 1..N
                      ┌───────▼───────┐
                      │  ParkingSpot   │
                      │ (Small/Med/Lg) │
                      └───────┬───────┘
                              │ 0..1
                      ┌───────▼───────┐
                      │   Vehicle      │
                      │ (Car/Truck/    │
                      │  Motorcycle)   │
                      └───────────────┘

┌──────────┐    ┌────────┐    ┌──────────────┐
│EntryPanel │───▶│ Ticket  │───▶│  ExitPanel    │
└──────────┘    └────────┘    └──────┬───────┘
                                      │
                              ┌───────▼───────┐
                              │   Payment      │
                              │ (Cash/Card)    │
                              └───────────────┘
```

**Step 3: Key Interfaces & Code**

```java
import java.time.Duration;
import java.time.LocalDateTime;
import java.util.*;
import java.util.stream.*;

// --- Enums ---
enum VehicleType { MOTORCYCLE, CAR, TRUCK }
enum SpotSize { SMALL, MEDIUM, LARGE }

// --- Strategy Pattern: Pricing ---
interface PricingStrategy {
    double calculateFee(double hours, SpotSize spotSize);
}

class HourlyPricing implements PricingStrategy {
    private static final Map<SpotSize, Double> RATES = Map.of(
        SpotSize.SMALL, 2.0, SpotSize.MEDIUM, 5.0, SpotSize.LARGE, 10.0
    );

    @Override
    public double calculateFee(double hours, SpotSize spotSize) {
        return RATES.get(spotSize) * Math.max(1, hours); // minimum 1 hour
    }
}

// --- Core Classes ---
class Vehicle {
    private final String licensePlate;
    private final VehicleType type;

    public Vehicle(String licensePlate, VehicleType type) {
        this.licensePlate = licensePlate;
        this.type = type;
    }

    public boolean fits(SpotSize spotSize) {
        return spotSize.ordinal() >= type.ordinal();
    }

    public String getLicensePlate() { return licensePlate; }
}

class ParkingSpot {
    private final String spotId;
    private final SpotSize size;
    private final int floor;
    private Vehicle vehicle;

    public ParkingSpot(String spotId, SpotSize size, int floor) {
        this.spotId = spotId;
        this.size = size;
        this.floor = floor;
    }

    public boolean isAvailable() { return vehicle == null; }

    public boolean park(Vehicle v) {
        if (isAvailable() && v.fits(size)) {
            this.vehicle = v;
            return true;
        }
        return false;
    }

    public void removeVehicle() { this.vehicle = null; }
    public SpotSize getSize() { return size; }
}

class Ticket {
    private final Vehicle vehicle;
    private final ParkingSpot spot;
    private final LocalDateTime entryTime;

    public Ticket(Vehicle vehicle, ParkingSpot spot) {
        this.vehicle = vehicle;
        this.spot = spot;
        this.entryTime = LocalDateTime.now();
    }

    public ParkingSpot getSpot() { return spot; }
    public LocalDateTime getEntryTime() { return entryTime; }
}

class ParkingLot {
    private static ParkingLot instance;
    private final Map<Integer, List<ParkingSpot>> floors;
    private final Map<String, Ticket> activeTickets = new HashMap<>();
    private PricingStrategy pricing = new HourlyPricing();

    private ParkingLot(Map<Integer, List<ParkingSpot>> floors) {
        this.floors = floors;
    }

    public static synchronized ParkingLot getInstance(Map<Integer, List<ParkingSpot>> floors) {
        if (instance == null) instance = new ParkingLot(floors);
        return instance;
    }

    private ParkingSpot findSpot(Vehicle vehicle) {
        return floors.keySet().stream().sorted()
            .flatMap(f -> floors.get(f).stream())
            .filter(s -> s.isAvailable() && vehicle.fits(s.getSize()))
            .findFirst().orElse(null);
    }

    public Ticket issueTicket(Vehicle vehicle) {
        ParkingSpot spot = findSpot(vehicle);
        if (spot == null) return null;
        spot.park(vehicle);
        Ticket ticket = new Ticket(vehicle, spot);
        activeTickets.put(vehicle.getLicensePlate(), ticket);
        return ticket;
    }

    public double processExit(String licensePlate) {
        Ticket ticket = activeTickets.remove(licensePlate);
        double hours = Duration.between(ticket.getEntryTime(), LocalDateTime.now()).toSeconds() / 3600.0;
        double fee = pricing.calculateFee(hours, ticket.getSpot().getSize());
        ticket.getSpot().removeVehicle();
        return fee;
    }
}
```

**Step 4: Sequence Diagram (Entry Flow)**
```
Driver → EntryPanel: scan/press button
EntryPanel → ParkingLot: find_spot(vehicle)
ParkingLot → ParkingFloor: iterate spots
ParkingFloor → ParkingSpot: is_available() && fits()
ParkingSpot → ParkingLot: return spot
ParkingLot → EntryPanel: issue_ticket(vehicle)
EntryPanel → Driver: print ticket, open gate
```

**Patterns used:** Singleton (ParkingLot), Strategy (PricingStrategy), Factory (could add VehicleFactory)

> **🧠 How I derived this:** Nouns from the problem → ParkingLot, Floor, Spot, Vehicle, Ticket, Payment. "What varies?" → Pricing (hourly vs flat vs weekend) → Strategy pattern. "Is there only one lot?" → Yes → Singleton. "Different vehicle types?" → Inheritance (Car/Truck/Motorcycle extend Vehicle). This derivation took 3 minutes, not memorization.

---

### ⭐ Fully Worked: LRU Cache (With Code)

> This is both an LLD AND a coding question. Be ready to implement it from scratch.

**Data Structure:** HashMap (O(1) lookup) + Doubly Linked List (O(1) insert/remove for recency tracking).

```java
import java.util.HashMap;
import java.util.Map;

public class LRUCache {
    private static class Node {
        int key, val;
        Node prev, next;
        Node(int key, int val) { this.key = key; this.val = val; }
    }

    private final int capacity;
    private final Map<Integer, Node> cache = new HashMap<>();
    private final Node head = new Node(0, 0); // dummy
    private final Node tail = new Node(0, 0); // dummy

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addToFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    public int get(int key) {
        if (!cache.containsKey(key)) return -1;
        Node node = cache.get(key);
        remove(node);
        addToFront(node); // mark as recently used
        return node.val;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) remove(cache.get(key));
        Node node = new Node(key, value);
        addToFront(node);
        cache.put(key, node);
        if (cache.size() > capacity) {
            Node lru = tail.prev; // least recently used
            remove(lru);
            cache.remove(lru.key);
        }
    }
}
```

**Complexity:** O(1) for both `get` and `put`. Space: O(capacity).

**Follow-up questions interviewers ask:**
- "Make it thread-safe" → Add a lock around get/put, or use read-write lock (multiple readers, single writer)
- "Add TTL support" → Store expiry timestamp in Node, lazy eviction on access + background cleanup thread
- "Distributed LRU?" → Consistent hashing to partition keys across cache nodes, each node runs local LRU

---

#### LLD-1. Design a Parking Lot

**Core classes:** `ParkingLot`, `ParkingFloor`, `ParkingSpot` (Small/Medium/Large), `Vehicle` (Car/Truck/Motorcycle), `Ticket`, `Payment`, `EntryPanel`, `ExitPanel`

**Patterns:** Strategy (pricing), Factory (vehicle/spot creation), Singleton (ParkingLot instance)

**Key logic:** Find nearest available spot matching vehicle size → issue ticket → on exit, calculate fee based on duration → process payment → free spot.

📖 [DesignGurus — Parking Lot](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-a-parking-lot)
📖 [AlgoMaster — Parking Lot](https://algomaster.io/learn/lld/design-parking-lot)
📖 [GeeksForGeeks — Parking Lot OOP](https://www.geeksforgeeks.org/system-design/design-parking-lot-using-object-oriented-principles/)

---

#### LLD-2. ⭐ Fully Worked: Elevator System (State + Strategy Patterns)

> This is the third most-asked LLD problem. It tests State pattern (elevator states), Strategy pattern (scheduling), and Observer pattern (displays) — all in one problem.

**Step 1: Requirements**
- Building with N floors and M elevators
- External requests: person on floor X wants to go UP or DOWN
- Internal requests: person inside elevator presses destination floor
- Scheduler picks the best elevator for each external request
- Elevator states: IDLE, MOVING_UP, MOVING_DOWN, DOOR_OPEN, MAINTENANCE

**Step 2: Core Classes & Relationships**

```
┌──────────────────┐       ┌──────────────┐
│ ElevatorSystem    │──────▶│  Scheduler    │ (Strategy pattern)
│                  │       │ (SCAN/LOOK)   │
└────────┬─────────┘       └──────────────┘
         │ 1..M
┌────────▼─────────┐       ┌──────────────┐
│    Elevator       │──────▶│  ElevatorState│ (State pattern)
│                  │       │ (Idle/Moving/ │
│                  │       │  DoorOpen)    │
└────────┬─────────┘       └──────────────┘
         │
┌────────▼─────────┐
│    Request        │
│ (floor, direction,│
│  type: INT/EXT)   │
└──────────────────┘
```

**Step 3: Key Interfaces & Code**

```java
import java.util.*;

// --- Enums ---
enum Direction {
    UP(1), DOWN(-1), IDLE(0);
    final int value;
    Direction(int v) { this.value = v; }
}

enum RequestType { EXTERNAL, INTERNAL }

// --- Request ---
class Request {
    final int floor;
    final Direction direction;
    final RequestType type;

    Request(int floor, Direction direction, RequestType type) {
        this.floor = floor;
        this.direction = direction;
        this.type = type;
    }
}

// --- State Pattern: Elevator behavior changes based on state ---
interface ElevatorState {
    void handleRequest(Elevator elevator, Request request);
}

class IdleState implements ElevatorState {
    @Override
    public void handleRequest(Elevator elevator, Request request) {
        if (request.floor == elevator.getCurrentFloor()) {
            elevator.openDoor();
        } else {
            elevator.setDirection(request.floor > elevator.getCurrentFloor() ? Direction.UP : Direction.DOWN);
            elevator.setState(new MovingState());
            elevator.addStop(request.floor);
        }
    }
}

class MovingState implements ElevatorState {
    @Override
    public void handleRequest(Elevator elevator, Request request) {
        elevator.addStop(request.floor); // Accept request if it's on the way (SCAN algorithm behavior)
    }
}

class DoorOpenState implements ElevatorState {
    @Override
    public void handleRequest(Elevator elevator, Request request) {
        elevator.addStop(request.floor); // queue it
    }
}

// --- Strategy Pattern: Scheduling algorithm is swappable ---
interface SchedulingStrategy {
    Elevator selectElevator(List<Elevator> elevators, Request request);
}

class LookStrategy implements SchedulingStrategy {
    /** LOOK algorithm: pick elevator closest to request floor, moving in same direction */
    @Override
    public Elevator selectElevator(List<Elevator> elevators, Request request) {
        Elevator best = null;
        int bestScore = Integer.MAX_VALUE;
        for (Elevator e : elevators) {
            if (e.isMaintenance()) continue;
            int distance = Math.abs(e.getCurrentFloor() - request.floor);
            int score;
            if (e.getDirection() == Direction.IDLE) {
                score = distance;
            } else if (e.getDirection() == request.direction) {
                boolean onTheWay = (e.getDirection() == Direction.UP && e.getCurrentFloor() <= request.floor)
                    || (e.getDirection() == Direction.DOWN && e.getCurrentFloor() >= request.floor);
                score = onTheWay ? distance : distance + 1000; // wrong side
            } else {
                score = distance + 2000; // opposite direction
            }
            if (score < bestScore) { bestScore = score; best = e; }
        }
        return best;
    }
}

// --- Elevator ---
class Elevator {
    private final int id;
    private int currentFloor = 0;
    private Direction direction = Direction.IDLE;
    private ElevatorState state = new IdleState();
    private final List<Integer> stops = new ArrayList<>();
    private boolean maintenance = false;

    Elevator(int id) { this.id = id; }

    public void addStop(int floor) {
        if (!stops.contains(floor)) {
            stops.add(floor);
            stops.sort(direction == Direction.DOWN ? Comparator.reverseOrder() : Comparator.naturalOrder());
        }
    }

    public void openDoor() {
        state = new DoorOpenState();
        stops.remove(Integer.valueOf(currentFloor));
    }

    public void move() {
        /** Called by system tick — moves elevator one floor */
        if (stops.isEmpty()) { direction = Direction.IDLE; state = new IdleState(); return; }
        state = new MovingState();
        currentFloor += direction.value;
        if (currentFloor == stops.get(0)) openDoor();
    }

    public int getCurrentFloor() { return currentFloor; }
    public Direction getDirection() { return direction; }
    public void setDirection(Direction d) { this.direction = d; }
    public void setState(ElevatorState s) { this.state = s; }
    public ElevatorState getState() { return state; }
    public boolean isMaintenance() { return maintenance; }
}

// --- System ---
class ElevatorSystem {
    private final List<Elevator> elevators;
    private final SchedulingStrategy strategy;

    ElevatorSystem(int numElevators, int numFloors, SchedulingStrategy strategy) {
        this.elevators = new ArrayList<>();
        for (int i = 0; i < numElevators; i++) elevators.add(new Elevator(i));
        this.strategy = strategy != null ? strategy : new LookStrategy();
    }

    public void requestElevator(Request request) {
        Elevator elevator = strategy.selectElevator(elevators, request);
        elevator.getState().handleRequest(elevator, request);
    }
}
```

**Step 4: Sequence Diagram (External Request)**
```
Person → FloorPanel: press UP on floor 5
FloorPanel → ElevatorSystem: request_elevator(Request(5, UP, EXTERNAL))
ElevatorSystem → Scheduler: select_elevator(elevators, request)
Scheduler → ElevatorSystem: return best elevator (e.g., Elevator #2)
ElevatorSystem → Elevator#2: state.handle_request(request)
Elevator#2 → Elevator#2: add_stop(5), set direction=UP
... (system tick moves elevator) ...
Elevator#2 → Elevator#2: arrive at floor 5, open_door()
Person → ElevatorPanel: press floor 10 (internal request)
ElevatorPanel → Elevator#2: state.handle_request(Request(10, UP, INTERNAL))
```

**Patterns used:** State (IdleState/MovingState/DoorOpenState), Strategy (LookStrategy — swappable with SCAN, SSTF), Observer (could add DisplayObserver for floor indicators)

**Follow-up questions:**
- "How do you handle rush hour?" → Priority queue for requests, express elevators for high floors
- "What about maintenance?" → `is_maintenance` flag, scheduler skips it, redistribute pending stops
- "How do you prevent starvation?" → Age-based priority — requests waiting >60s get boosted priority

📖 [DesignGurus — Elevator](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-an-elevator-system)

---

#### LLD-3. Design a Chess Game

**Core classes:** `Game`, `Board`, `Cell`, `Piece` (King/Queen/Rook/Bishop/Knight/Pawn), `Player`, `Move`, `Color`

**Patterns:** Strategy (each piece type has its own movement strategy), Command (moves can be undone)

**Key logic:** Validate move based on piece rules → check if move puts own king in check → execute move → check for checkmate/stalemate.

📖 [DesignGurus — Chess](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-chess)

---

#### LLD-4. Design Tic-Tac-Toe

**Core classes:** `Game`, `Board`, `Cell`, `Player`, `Symbol` (X/O)

**Key logic:** Players alternate turns → place symbol → check win condition (row/col/diagonal) → check draw (board full).

**Optimization:** Track row/col/diagonal sums. Player X = +1, Player O = -1. If any sum reaches +N or -N, that player wins. O(1) win check.

---

#### LLD-5. Design an LRU Cache

**Core classes:** `LRUCache`, `Node` (doubly-linked list node), `HashMap`

**Data structure:** HashMap (O(1) lookup) + Doubly Linked List (O(1) insert/remove for recency tracking).

**Operations:**
- `get(key)`: If exists, move to front of list, return value. Else return -1.
- `put(key, value)`: If exists, update and move to front. If full, evict tail (least recently used). Insert at front.

**This is also a coding question** — be ready to implement it in code.

---

#### LLD-6. Design a Vending Machine

**Core classes:** `VendingMachine`, `Product`, `Slot`, `Coin`/`Payment`, `Inventory`

**Patterns:** State (idle → coin_inserted → product_selected → dispensing), Strategy (payment methods)

**States:** NoCoin → HasCoin → ProductSelected → Dispensing → NoCoin

---

#### LLD-7. Design a Library Management System

**Core classes:** `Library`, `Book`, `BookCopy`, `Member`, `Librarian`, `Loan`, `Reservation`, `Fine`, `Catalog`

**Patterns:** Observer (notify when reserved book available), Strategy (fine calculation)

**Key logic:** Search catalog → check availability → issue book → track due date → calculate fine on late return → handle reservations.

📖 [DesignGurus — Library](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-a-library-management-system)

---

#### LLD-8. Design a Splitwise (Expense Sharing)

**Core classes:** `User`, `Group`, `Expense`, `Split` (Equal/Exact/Percentage), `Balance`

**Key logic:** Track who owes whom. Simplify debts algorithm (min cash flow — model as a graph, find net balance per person, greedily settle largest debtor with largest creditor).

**Patterns:** Strategy (split calculation — equal, exact, percentage), Observer (notify users of new expenses)

---

### Tier 2: High Frequency

#### LLD-9. Design a Hotel Booking System

**Core classes:** `Hotel`, `Room`, `RoomType` (Single/Double/Suite), `Reservation`, `Guest`, `Payment`, `DateRange`

**Patterns:** Strategy (pricing — weekday/weekend/seasonal), State (reservation: Pending → Confirmed → CheckedIn → CheckedOut → Cancelled)

**Key logic:** Search available rooms for date range → check no overlapping reservations → create reservation with 15-min payment timeout → confirm on payment → handle cancellation with refund policy.

**Tricky part:** Date-range availability check. For each room, check that no existing reservation overlaps with the requested range: `NOT (existing.end <= requested.start OR existing.start >= requested.end)`. Use an interval-based query or pre-computed availability calendar.

**Edge cases:** Overbooking strategy (hotels overbook by 5-10% expecting cancellations), concurrent booking of same room (optimistic locking on room+date), timezone handling for check-in/check-out.

---

#### LLD-10. Design a Movie Ticket Booking System

**Core classes:** `Cinema`, `Hall`, `Show` (movie + hall + time), `Seat`, `Booking`, `Payment`, `SeatLock`

**Patterns:** State (booking: SeatSelected → PaymentPending → Confirmed → Cancelled), Strategy (pricing — regular/premium/VIP)

**Key logic:** User selects seats → acquire temporary lock (5-min timeout in Redis) → process payment → confirm booking → release lock. If payment times out, release seats automatically.

**Tricky part:** Concurrent seat selection. Two users select the same seat simultaneously. Solution: Optimistic locking — `UPDATE seats SET status='LOCKED', locked_by=? WHERE id=? AND status='AVAILABLE'`. Only one succeeds. The other gets "seat unavailable."

**Edge cases:** Bulk booking (10 seats together), seat map rendering with real-time availability, refund on cancellation within policy window.

📖 [DesignGurus — Movie Ticket](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-a-movie-ticket-booking-system)

---

#### LLD-11. Design an In-Memory File System

**Core classes:** `FileSystem`, `Entry` (abstract), `File extends Entry`, `Directory extends Entry`, `Path`

**Patterns:** Composite (Directory contains list of Entry — both Files and Directories), Iterator (traverse directory tree)

**Key logic:** Directory holds `Map<String, Entry>` of children. Path resolution: split path by `/`, traverse tree from root. CRUD: `mkdir`, `touch`, `ls`, `rm`, `cat`, `mv`. `ls` on directory returns children names. `cat` on file returns content.

**Tricky part:** Path resolution with `.` (current) and `..` (parent). Use a stack: push on directory name, pop on `..`, ignore `.`. Final stack = canonical path.

---

#### LLD-12. Design a Logging Framework

**Core classes:** `Logger` (Singleton), `LogLevel` (DEBUG/INFO/WARN/ERROR/FATAL), `LogHandler` (ConsoleHandler/FileHandler/RemoteHandler), `LogFormatter` (JSONFormatter/TextFormatter), `LogMessage`

**Patterns:** Singleton (one Logger instance), Chain of Responsibility (log message passes through handlers — console AND file AND remote), Strategy (formatter is swappable), Builder (LogMessage construction)

**Key logic:** `Logger.log(level, message)` → check if level >= configured threshold → create LogMessage → pass through chain of handlers → each handler formats using its Formatter and outputs. Handlers can be added/removed at runtime.

---

#### LLD-13. Design a Pub/Sub System (In-Process)

**Core classes:** `EventBus` (Singleton), `Topic`, `Publisher`, `Subscriber` (interface with `onMessage()`), `Message`

**Patterns:** Observer (subscribers notified on new messages), Singleton (one EventBus)

**Key logic:** `EventBus` maintains `Map<String, List<Subscriber>>`. `subscribe(topic, subscriber)` adds to list. `publish(topic, message)` iterates subscribers and calls `onMessage()`. For async: use a thread pool to notify subscribers in parallel. For ordering: use a single-threaded executor per topic.

**Edge cases:** Subscriber throws exception (catch and continue to next subscriber), unsubscribe during iteration (use CopyOnWriteArrayList or snapshot), message filtering (subscribers specify a predicate).

---

#### LLD-14. Design a Task Scheduler

**Core classes:** `Scheduler`, `Task`, `Trigger` (OneTimeTrigger/CronTrigger), `Worker`, `TaskQueue` (PriorityQueue sorted by next execution time)

**Patterns:** Strategy (trigger types — one-time, recurring, cron), Observer (notify on task completion/failure), Command (Task encapsulates executable action)

**Key logic:** Scheduler runs a loop: peek at PriorityQueue → if top task's `nextExecutionTime <= now`, dequeue and submit to thread pool → on completion, if recurring, calculate next execution time and re-enqueue. Use `DelayQueue` or `ScheduledExecutorService` in Java.

**Tricky part:** Cron expression parsing, handling task failures (retry with backoff, dead-letter after N failures), preventing duplicate execution in distributed setting (distributed lock on task ID).

---

#### LLD-15. Design a Shopping Cart

**Core classes:** `Cart`, `CartItem`, `Product`, `User`, `Coupon`, `PriceCalculator`, `DiscountStrategy`

**Patterns:** Strategy (discount calculation — percentage, flat, buy-one-get-one), Observer (price updates when cart changes)

**Key logic:** `addItem(product, qty)` → check inventory → add to cart → recalculate total. `applyCoupon(code)` → validate coupon (not expired, min order met, usage limit) → apply DiscountStrategy → recalculate. `checkout()` → validate inventory again → create order → clear cart.

**Edge cases:** Item goes out of stock while in cart, price changes between add-to-cart and checkout, multiple coupons (stackable vs exclusive), cart expiration for guest users.

---

#### LLD-16. Design an ATM Machine

**Core classes:** `ATM`, `Account`, `Card`, `Transaction` (Withdrawal/Deposit/BalanceInquiry), `CashDispenser`, `Screen`, `ATMState`

**Patterns:** State (Idle → CardInserted → PinEntered → TransactionSelected → Processing → Idle), Chain of Responsibility (cash dispensing — $100 bills first, then $50, then $20)

**Key logic:** Insert card → validate card → enter PIN (3 attempts, lock on failure) → select transaction → process → dispense cash → eject card. CashDispenser uses greedy algorithm: dispense largest denomination first.

**Tricky part:** Concurrent access (two ATMs accessing same account — use DB-level locking on account), partial dispensing failure (dispensed $200 of $500 — rollback transaction, alert maintenance), daily withdrawal limits.

📖 [DesignGurus — ATM](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-an-atm)

---

#### LLD-17. Design a Food Ordering System

**Core classes:** `Restaurant`, `Menu`, `MenuItem`, `Order`, `OrderItem`, `Customer`, `DeliveryAgent`, `Payment`, `OrderState`

**Patterns:** State (order: Placed → Confirmed → Preparing → ReadyForPickup → OutForDelivery → Delivered → Cancelled), Strategy (delivery fee calculation — distance-based, flat, free above threshold), Observer (notify customer on state changes)

**Key logic:** Customer browses restaurants → adds items to cart → places order → restaurant confirms → assigns delivery agent (nearest available) → track delivery via real-time location updates. Key: order state machine with valid transitions only (can't go from Delivered back to Preparing).

**Edge cases:** Restaurant rejects order (out of stock), delivery agent cancels (reassign), partial order fulfillment, refund on cancellation based on order state.

---

### Tier 3: Occasionally Asked

#### LLD-18. Design a Snake and Ladder Game

**Core classes:** `Game`, `Board`, `Cell`, `Snake` (head→tail), `Ladder` (bottom→top), `Player`, `Dice`

**Key logic:** Player rolls dice → move forward → if snake head, slide down to tail → if ladder bottom, climb to top → first to reach cell 100 wins. Handle: multiple players, configurable board, edge case (exact landing on 100 vs overshoot).

**Patterns:** Strategy (dice — single die, double dice, loaded dice), Template Method (turn sequence: roll → move → check snake/ladder → check win)

📖 [AlgoMaster — Snake and Ladder](https://algomaster.io/learn/lld)

---

#### LLD-19. Design a Calendar / Meeting Scheduler

**Core classes:** `Calendar`, `Event`, `TimeSlot`, `User`, `RecurrenceRule`, `ConflictChecker`, `Room` (optional)

**Key logic:** Create event with start/end time → check conflicts with existing events → handle recurring events (daily/weekly/monthly via RRULE) → find free slots across multiple users (intersection of available times). Key challenge: efficient overlap detection using interval tree or sorted events with binary search.

**Patterns:** Observer (notify attendees of changes), Strategy (recurrence calculation — daily, weekly, custom), Builder (complex event creation with optional fields)

**Interview talking point:** "I'd store events sorted by start time per user. To find conflicts, I binary search for overlapping intervals. For 'find common free time across N users,' I merge all busy intervals and invert to get free slots."

---

#### LLD-20. Design an Online Auction System

**Core classes:** `Auction`, `Item`, `Bid`, `User` (Buyer/Seller), `AuctionState` (Open/Closed/Cancelled), `Timer`, `NotificationService`

**Key logic:** Seller creates auction with reserve price and end time → buyers place bids (must exceed current highest) → timer expires → highest bid ≥ reserve price wins → notify winner and seller. Key challenge: concurrent bid handling (optimistic locking — `UPDATE bids SET highest = new_bid WHERE highest = old_bid`).

**Patterns:** State (auction lifecycle: Draft → Active → Closed → Settled), Observer (notify bidders of new highest bid), Strategy (auction types: English ascending, Dutch descending, sealed-bid)

---

#### LLD-21. Design a Ride-Sharing System (Class Level)

**Core classes:** `RideService`, `Rider`, `Driver`, `Ride`, `Location`, `RideStatus`, `FareCalculator`, `MatchingStrategy`

**Patterns:** Strategy (matching — nearest driver, highest rated, lowest ETA), State (ride: Requested → DriverAssigned → InProgress → Completed → Cancelled), Observer (notify rider of driver location updates)

**Key logic:** Rider requests ride with pickup/dropoff → MatchingStrategy finds best driver within radius → driver accepts/rejects (timeout = reject → try next) → ride state transitions → fare calculated on completion using distance + time + surge multiplier.

**Tricky part:** Driver matching with concurrent requests. Two riders request at the same time, both match to the same driver. Solution: optimistic lock on driver status — `UPDATE drivers SET status='ASSIGNED' WHERE id=? AND status='AVAILABLE'`. Only one succeeds.

**Edge cases:** Driver cancels mid-ride (reassign), rider cancels after driver assigned (cancellation fee), surge pricing calculation, driver goes offline during matching.

---

#### LLD-22. Design a Notification Service (Class Level)

**Core classes:** `NotificationService`, `Notification`, `NotificationChannel` (Push/SMS/Email), `NotificationTemplate`, `UserPreference`, `NotificationPriority`, `RetryPolicy`

**Patterns:** Strategy (channel selection — push, SMS, email each have different send logic), Observer (subscribers register for notification types), Template Method (all channels follow: validate → render template → send → log), Factory (create channel-specific sender)

**Key logic:** `send(userId, type, data)` → check UserPreference (opted in for this type+channel?) → select channels → render template with data → enqueue per channel (priority queue — OTP > transactional > marketing) → channel worker sends → on failure, retry with backoff → after N failures, dead-letter queue.

**Tricky part:** Rate limiting per user (max 5 notifications/hour for marketing), deduplication (same notification sent twice due to retry — use idempotency key), batching (digest mode — collect notifications for 5 min, send as one).

**Edge cases:** User has no push token (fallback to SMS), template rendering fails (log and skip, don't crash), timezone-aware delivery (don't send marketing at 3 AM).

---

#### LLD-23. Design a Rate Limiter (Implementation)

**Core classes:** `RateLimiter` (interface), `TokenBucketLimiter`, `SlidingWindowLimiter`, `RateLimitConfig`, `RateLimitResult`

**Patterns:** Strategy (algorithm is swappable — token bucket, sliding window, fixed window), Factory (create limiter based on config)

**Key logic — Token Bucket:**
```
class TokenBucketLimiter:
  - tokens: current token count
  - maxTokens: bucket capacity
  - refillRate: tokens added per second
  - lastRefillTime: timestamp

  allowRequest():
    refill()  // add tokens based on elapsed time
    if tokens >= 1:
      tokens -= 1
      return ALLOWED
    else:
      return REJECTED (with retryAfter header)

  refill():
    elapsed = now - lastRefillTime
    newTokens = elapsed * refillRate
    tokens = min(maxTokens, tokens + newTokens)
    lastRefillTime = now
```

**Tricky part:** Thread safety — multiple threads calling `allowRequest()` concurrently. Use `synchronized` or `AtomicInteger` with CAS. For distributed rate limiting, use Redis `INCR` + `EXPIRE` atomically via Lua script.

**Edge cases:** Clock skew in distributed systems, handling rate limit headers (`X-RateLimit-Remaining`, `X-RateLimit-Reset`, `Retry-After`), different limits per endpoint/user tier.

---

#### LLD-24. Design a Thread-Safe Circular Buffer

**Core classes:** `CircularBuffer<T>`, internal array, `head` pointer, `tail` pointer, `size`, `capacity`

**Patterns:** None needed — this is a data structure problem. Focus on correctness and thread safety.

**Key logic:**
```
class CircularBuffer<T>:
  - array[capacity]
  - head = 0  // read position
  - tail = 0  // write position
  - size = 0
  - lock: ReentrantLock (or ReadWriteLock)

  put(item):
    lock.lock()
    if size == capacity: throw BufferFullException (or overwrite oldest)
    array[tail] = item
    tail = (tail + 1) % capacity
    size++
    lock.unlock()

  get():
    lock.lock()
    if size == 0: throw BufferEmptyException
    item = array[head]
    head = (head + 1) % capacity
    size--
    lock.unlock()
    return item
```

**Tricky part:** Blocking vs non-blocking behavior. Blocking version: `put()` blocks when full (producer waits), `get()` blocks when empty (consumer waits). Use `Condition` variables (`notFull.await()`, `notEmpty.signal()`). This is the classic bounded buffer / producer-consumer problem.

**Edge cases:** Single-element buffer, buffer wraps around (head > tail), concurrent put+get, graceful shutdown (drain remaining items).

---

#### LLD-25. Design a Cache with TTL Support

**Core classes:** `TTLCache<K,V>`, `CacheEntry` (value + expiryTimestamp), `CleanupStrategy`

**Patterns:** Strategy (cleanup — lazy eviction vs background thread vs hybrid), Decorator (add TTL on top of existing LRU cache)

**Key logic:**
```
class TTLCache<K,V>:
  - map: HashMap<K, CacheEntry>
  - defaultTTL: Duration

  put(key, value, ttl):
    entry = CacheEntry(value, now + ttl)
    map.put(key, entry)

  get(key):
    entry = map.get(key)
    if entry == null: return null
    if entry.expiryTime < now:  // lazy eviction
      map.remove(key)
      return null
    return entry.value
```

**Cleanup strategies:**
- **Lazy eviction:** Check expiry on `get()`. Simple but expired keys consume memory until accessed.
- **Background thread:** Periodic sweep (every 30s) removes expired keys. Frees memory but adds CPU overhead.
- **Hybrid (Redis approach):** Lazy eviction on access + background sweep of random sample (check 20 random keys, delete expired, repeat if >25% were expired).

**Tricky part:** Thread safety for concurrent `put`/`get`/cleanup. Use `ConcurrentHashMap` + atomic operations, or `ReadWriteLock`. Background cleanup thread must not block readers.

**Follow-up:** "Combine this with LRU eviction" → When cache is full AND no keys are expired, evict least recently used. This is LRU + TTL — the most common real-world cache configuration.

---

## Part 4: Design Patterns

> Don't memorize all 23 GoF patterns. These are the ones that actually appear in LLD interviews.

📖 [Refactoring.Guru — All Patterns with Examples](https://refactoring.guru/design-patterns)

---

### Creational Patterns

#### 1. Singleton
**What:** Ensures a class has exactly one instance with a global access point.
**Use in interviews:** Logger, Configuration, Database connection pool, ParkingLot.
**Code idea:** Private constructor + static `getInstance()` method. Thread-safe with double-checked locking.
📖 [Refactoring.Guru — Singleton](https://refactoring.guru/design-patterns/singleton)

#### 2. Factory / Abstract Factory
**What:** Creates objects without specifying the exact class. Caller says "give me a Vehicle" and factory decides Car vs Truck.
**Use in interviews:** Creating different vehicle types, payment processors, notification channels.
📖 [Refactoring.Guru — Factory](https://refactoring.guru/design-patterns/factory-method)

#### 3. Builder
**What:** Constructs complex objects step by step. Separates construction from representation.
**Use in interviews:** Building complex query objects, notification messages, configuration objects.
📖 [Refactoring.Guru — Builder](https://refactoring.guru/design-patterns/builder)

---

### Behavioral Patterns

#### 4. Strategy
**What:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.
**Use in interviews:** Pricing strategies (parking lot), sorting algorithms, payment methods, routing algorithms.
**Example:** `PricingStrategy` interface → `HourlyPricing`, `FlatRatePricing`, `WeekendPricing`.
📖 [Refactoring.Guru — Strategy](https://refactoring.guru/design-patterns/strategy)

#### 5. Observer
**What:** When one object changes state, all dependents are notified automatically.
**Use in interviews:** Pub/Sub, notification systems, event listeners, stock price updates, elevator displays.
📖 [Refactoring.Guru — Observer](https://refactoring.guru/design-patterns/observer)

#### 6. Command
**What:** Encapsulates a request as an object, allowing undo/redo, queuing, and logging.
**Use in interviews:** Chess moves (undo), text editor operations, task scheduling.
📖 [Refactoring.Guru — Command](https://refactoring.guru/design-patterns/command)

#### 7. State
**What:** Object changes behavior when its internal state changes. Appears as if the object changed its class.
**Use in interviews:** Vending machine states, elevator states, order status (placed → confirmed → shipped → delivered).
📖 [Refactoring.Guru — State](https://refactoring.guru/design-patterns/state)

#### 8. Iterator *(Rarely asked — know the concept, skip deep prep)*
**What:** Provides a way to access elements of a collection sequentially without exposing its underlying representation. Most languages have built-in iterators, so this rarely comes up as a standalone interview question.
📖 [Refactoring.Guru — Iterator](https://refactoring.guru/design-patterns/iterator)

---

### Structural Patterns

#### 9. Decorator
**What:** Adds behavior to objects dynamically by wrapping them. Alternative to subclassing.
**Use in interviews:** Adding toppings to pizza, adding features to a notification (SMS + email + push).
📖 [Refactoring.Guru — Decorator](https://refactoring.guru/design-patterns/decorator)

#### 10. Adapter
**What:** Converts the interface of a class into another interface clients expect. Makes incompatible interfaces work together.
**Use in interviews:** Integrating third-party payment gateways, legacy system integration.
📖 [Refactoring.Guru — Adapter](https://refactoring.guru/design-patterns/adapter)

---

### Architectural Patterns (For HLD)

#### 11. MVC / MVVM *(Rarely asked in backend SDE2 rounds — know the concept, skip deep prep)*
Model-View-Controller. Separates data, presentation, and control logic. Primarily relevant for frontend/mobile roles. In backend SDE2 interviews, you're more likely to discuss Repository pattern, CQRS, or Event Sourcing. Mention MVC only if the interviewer asks about frontend architecture.

#### 12. Repository Pattern
Abstracts data access. Service layer talks to repository, not directly to DB.

#### 13. Event Sourcing *(🔶 STRETCH — SDE3/Staff pattern)*
Store all changes as a sequence of events rather than current state. Enables full audit trail and replay. Know the name; don't prep the implementation.

#### 14. CQRS (Command Query Responsibility Segregation) *(🔶 STRETCH — SDE3/Staff pattern)*
Separate read and write models. Write model optimized for writes, read model (often denormalized) optimized for queries. Often paired with event sourcing. Know the name; don't prep the implementation.

---

### SOLID Principles (Know These Cold)

| Principle | Meaning | Interview Example |
|-----------|---------|-------------------|
| **S** — Single Responsibility | A class should have one reason to change | `PaymentProcessor` shouldn't also send emails |
| **O** — Open/Closed | Open for extension, closed for modification | Add new `PricingStrategy` without changing `ParkingLot` |
| **L** — Liskov Substitution | Subtypes must be substitutable for their base types | `ElectricCar` should work wherever `Vehicle` is expected |
| **I** — Interface Segregation | Don't force clients to depend on interfaces they don't use | Separate `Flyable` and `Swimmable` instead of one `Animal` interface |
| **D** — Dependency Inversion | Depend on abstractions, not concretions | `NotificationService` depends on `MessageSender` interface, not `SMSSender` directly |

---

## Part 5: The Interview Framework + Trade-off Mastery

### The 4-Step Framework (Use for EVERY HLD Interview)

```
┌─────────────────────────────────────────────────────────┐
│  Step 1: REQUIREMENTS (5 min)                           │
│  • Functional: What should the system DO?               │
│  • Non-functional: Scale, latency, availability,        │
│    consistency                                          │
│  • Back-of-envelope: QPS, storage, bandwidth            │
│  • Confirm scope with interviewer                       │
├─────────────────────────────────────────────────────────┤
│  Step 2: HIGH-LEVEL DESIGN (10 min)                     │
│  • Draw core components: Client → LB → API → DB/Cache  │
│  • Define APIs (REST endpoints with proper naming)      │
│  • Define data model (schema driven by access patterns) │
│  • Show read path and write path                        │
├─────────────────────────────────────────────────────────┤
│  Step 3: DEEP DIVE (20 min)                             │
│  • Ask interviewer: "Which area should I dive into?"    │
│  • Pick 2-3 critical components                         │
│  • Discuss: DB choice, sharding, caching, algorithms    │
│  • Address failure modes and edge cases                 │
│  • Discuss trade-offs with alternatives (use framework) │
├─────────────────────────────────────────────────────────┤
│  Step 4: WRAP UP (5 min)                                │
│  • Verify design meets requirements                     │
│  • Identify bottlenecks and SLIs to monitor             │
│  • Mention monitoring/alerting (p99, error rate, QPS)   │
│  • Suggest future extensions                            │
│  • State cost considerations if relevant                │
└─────────────────────────────────────────────────────────┘
```

### The Trade-off Discussion Framework

> This is the single most important skill in a system design interview. Interviewers in 2025-2026 care more about HOW you discuss trade-offs than whether you pick the "right" answer.

**The Template (memorize this):**

```
"I considered [Option A] and [Option B].
Option A gives us [specific benefit] but costs us [specific drawback].
Option B gives us [specific benefit] but costs us [specific drawback].
For THIS use case, I'd go with [choice] because [reason tied to our requirements]."
```

**Example in action:**

*Bad:* "I'd use Kafka."

*Good:* "For the message queue, I considered SQS and Kafka. SQS is simpler to operate and fully managed — we don't need to manage brokers or partitions. Kafka gives us higher throughput, message replay, and ordering guarantees per partition. Since our notification system needs at-least-once delivery but doesn't need replay or strict ordering, SQS is the better fit — it's simpler and cheaper for this use case."

**Common trade-off axes to discuss:**

| Axis | Option A | Option B | How to Decide |
|------|----------|----------|---------------|
| Consistency vs Availability | CP (reject requests during partition) | AP (serve stale data) | Payment = CP. Feed = AP. |
| Latency vs Throughput | Optimize for single-request speed | Optimize for batch processing | User-facing = latency. Analytics = throughput. |
| Cost vs Performance | Cheaper infra, higher latency | Expensive infra, lower latency | Start cheap, optimize when SLOs are breached. |
| Simplicity vs Scalability | Monolith, single DB | Microservices, sharded DB | Start simple. Complexity only when scale demands it. |
| Read vs Write optimization | Denormalize (fast reads, slow writes) | Normalize (fast writes, slow reads) | Read-heavy (100:1) = denormalize. Write-heavy = normalize. |
| Push vs Pull | Fan-out-on-write (pre-compute) | Fan-out-on-read (compute on demand) | Small fan-out = push. Celebrity fan-out = pull. |

### Red Flags That Instantly Signal "Junior" to Interviewers

> Based on real interviewer feedback from DesignGurus, Blind, and interviewing.io data.

🚩 **Jumping to boxes without requirements** — "Let me draw the architecture" before asking a single clarifying question. Interviewers see this as inability to scope a problem.

🚩 **Name-dropping without justification** — "I'd use Kafka, Redis, Elasticsearch, and DynamoDB" without explaining WHY each is needed. This signals memorization, not understanding.

🚩 **Designing for Google-scale when the prompt says "startup"** — If the interviewer says "10K users," don't design for 1 billion. Match your design to the stated requirements.

🚩 **Ignoring non-functional requirements** — Never discussing latency, availability, consistency, or cost. These are what separate SDE2 from SDE1.

🚩 **Single-option thinking** — Presenting one solution without mentioning alternatives. Always show you considered at least 2 options.

🚩 **Forgetting failure modes** — "What if this component goes down?" If you don't address this proactively, the interviewer will ask, and you'll look reactive instead of proactive.

🚩 **Over-engineering from the start** — Adding CQRS, event sourcing, and microservices for a simple CRUD app. Start simple, add complexity only when requirements demand it.

🚩 **No monitoring story** — Finishing a design without mentioning how you'd know if it's healthy. Always end with: "I'd monitor p99 latency, error rate, and [system-specific metric]."

### Key Signals Interviewers Look For at SDE2

✅ **Do:**
- Drive the conversation — don't wait for hints
- Ask clarifying questions before designing
- Do back-of-envelope math
- Articulate trade-offs ("I chose X over Y because...")
- Consider failure modes proactively
- Start simple, then iterate
- Ask "Is there a specific area you'd like me to dive deeper into?"

❌ **Don't:**
- Jump straight into drawing boxes
- Name-drop technologies without explaining why
- Design for unrealistic scale without grounding in requirements
- Ignore non-functional requirements
- Present only one option without discussing alternatives
- Forget about monitoring and operational concerns

### 📋 Mock Interview Self-Evaluation Rubric

> Use this after every mock interview. Score yourself honestly. Target: 7+ in each category for a Strong Hire.

**HLD Mock Rubric (45 minutes):**

| Category | Score 1-3 (No Hire) | Score 4-6 (Lean Hire) | Score 7-10 (Strong Hire) |
|----------|--------------------|-----------------------|--------------------------|
| **Requirements** (5 min) | Jumped to design without clarifying | Asked 1-2 questions, missed non-functional | 3+ functional + 2+ non-functional + back-of-envelope math |
| **High-Level Design** (10 min) | Missing major components | Drew components but no APIs or data model | Complete diagram + APIs + data model + read/write paths |
| **Deep Dive** (20 min) | Surface-level, no alternatives | Discussed 1 component with some depth | 2-3 components, alternatives considered, failure modes addressed |
| **Trade-offs** | "I'd use Redis" (no justification) | Mentioned one alternative | "I considered X and Y, chose X because..." for 2+ decisions |
| **Communication** | Waited for hints, silent thinking | Mostly drove but needed nudges | Drove conversation, asked "which area to dive into?", structured thinking |
| **Wrap-up** (5 min) | No wrap-up | Mentioned monitoring | Bottlenecks + monitoring (p99, error rate) + future extensions |

**LLD Mock Rubric (40 minutes):**

| Category | Score 1-3 | Score 4-6 | Score 7-10 |
|----------|-----------|-----------|------------|
| **Class Modeling** | Random classes, unclear relationships | Core classes identified, some relationships | Clean classes from nouns, clear has-a/is-a relationships |
| **Design Patterns** | No patterns or force-fitted | 1 pattern correctly applied | 2+ patterns naturally applied (Strategy, State, Observer) |
| **APIs/Methods** | Vague method names | Methods defined but signatures incomplete | Clean method signatures with params and return types |
| **Edge Cases** | None mentioned | 1-2 mentioned when prompted | Proactively addressed 3+ edge cases |
| **SOLID Principles** | Violated SRP, tight coupling | Mostly clean, minor violations | Clear separation of concerns, extensible design |

**After each mock, ask yourself:**
1. Where did I spend too much time? (Adjust pacing)
2. What question caught me off guard? (Study that concept)
3. Did I use the trade-off template? ("I considered X and Y...")
4. Did I mention monitoring in my wrap-up?

---

## Part 6: Company-Specific Notes

### Amazon (SDE2 — L5)
- **Rounds:** Typically 4 onsite rounds: 3 coding rounds (DSA + Leadership Principles) + 1 system design round with the hiring manager. The coding rounds are categorized as: (1) Data Structures & Algorithms, (2) Logical & Maintainable (often includes OOD/LLD elements), (3) Problem Solving. Some teams do 2 SD rounds instead. Always confirm structure with your recruiter.
- **Style:** Expect AWS service awareness (DynamoDB, S3, SQS, Lambda, EC2). You don't need to be an AWS expert, but knowing when to use DynamoDB vs Aurora shows practical judgment.
- **Unique:** Leadership Principles bleed into design rationale. When you say "I chose X over Y," the interviewer is also evaluating Dive Deep, Bias for Action, Are Right A Lot. Frame your trade-offs accordingly.
- **The "Logical & Maintainable" round:** This is a coding round but often overlaps with LLD — you may be asked to design classes, apply OOP principles, and write clean, extensible code. Prepare for this as a hybrid coding+LLD round.
- **Favorites:** Parking lot, URL shortener, e-commerce, notification system, warehouse system, food delivery

**How to Weave Leadership Principles into System Design (Amazon-Specific):**

| When You Say This... | LP Being Evaluated | How to Frame It |
|----------------------|-------------------|-----------------|
| "I'd start with a monolith and extract microservices later" | **Bias for Action** + **Frugality** | "I'd rather ship a working system quickly than over-engineer upfront. We can refactor when we have real traffic data." |
| "Let me calculate the QPS and storage before choosing a DB" | **Dive Deep** | "I want to make a data-driven decision rather than defaulting to a technology." |
| "I considered X and Y, and chose X because..." | **Are Right, A Lot** | Show you weighed options with good judgment, not just picked the popular choice. |
| "If this component fails, here's how we recover..." | **Insist on the Highest Standards** | Proactively addressing failure modes shows operational maturity. |
| "This design handles 10x growth without re-architecture" | **Think Big** | Show you're designing for the future, not just today's requirements. |
| "We can use S3 IA instead of Standard to save 45% on storage" | **Frugality** | Cost awareness is a real LP signal at Amazon. |
| "I'd add monitoring for p99 latency and set up PagerDuty alerts" | **Ownership** | You own the system end-to-end, including operations. |
- 📖 [IGotAnOffer — Amazon SD Guide](https://igotanoffer.com/en/advice/amazon-system-design-interview)

### Google (L4)
- **Rounds:** System design round is **not guaranteed at L4** — it depends on the team and hiring committee. Some L4 loops have 0 SD rounds (all coding), others have 1. Always confirm with your recruiter.
- **Expectations when SD is present:** Significantly lower than L5. They want clean, maintainable architecture — not planet-scale distributed systems. Focus on clear requirements, simple trade-offs, and structured thinking. You're expected to "build out solutions to well-defined problems" (per ex-Google senior EM feedback).
- **Style:** Prefer you build from primitives. Don't say "use Redis" — explain what you need (in-memory hash map with TTL and LRU eviction) and why.
- **Unique:** May ask you to design internal Google products (YouTube, Maps, Photos). Open-ended prompts with emphasis on consistency and correctness.
- **Favorites:** YouTube, Google Maps, distributed cache, autocomplete, web crawler
- **Key difference from L5:** L4 can ask for hints and still pass. L5 must drive independently. L4 designs for thousands-to-millions of users, not billions.
- 📖 [IGotAnOffer — Google SD Guide](https://igotanoffer.com/blogs/tech/google-system-design-interview)
- 📖 [SystemDesignHandbook — Google L4 SD](https://www.systemdesignhandbook.com/guides/google-l4-system-design/)

### Meta (E4)
- **Rounds:** 1 system design round (you may choose between system design and product architecture)
- **Style:** Focus on scale (billions of users). Feed ranking is a favorite topic.
- **Unique:** Product architecture option focuses on API design, data modeling for user-facing products
- **Favorites:** Instagram, WhatsApp, news feed, live commenting, typeahead
- 📖 [IGotAnOffer — Meta SD Guide](https://igotanoffer.com/blogs/tech/meta-system-design-interview)

### Microsoft
- **Rounds:** 1-2 system design rounds
- **Style:** OOP/LLD is heavily tested. Azure awareness helps.
- **Favorites:** Shopping cart, online portal, collaborative editor

### Apple
- **Rounds:** 1 system design round
- **Style:** Clean, modular thinking valued. Elevator-style problems.
- **Favorites:** Elevator system, smart home, blackjack

### Uber
- **Rounds:** 1-2 system design rounds
- **Style:** Geo-spatial, real-time systems dominate.
- **Favorites:** Ride matching, driver heat map, trip tracking, surge pricing

### Netflix
- **Style:** Streaming, CDN, recommendation systems.
- **Favorites:** Video streaming, content recommendation, A/B testing platform

### Anthropic / OpenAI
- **Style:** LLM inference systems, distributed search, batched processing.
- **Favorites:** LLM-powered search, batched inference, chat service design

### Remote-First & High-Paying Startups (Stripe, Shopify, Datadog, Cloudflare, Coinbase)

> These companies pay FAANG-level or higher, often with remote flexibility. Their interviews differ significantly from FAANG.

**How they differ from FAANG:**
- **Pragmatic over theoretical** — "Design this for a 10-person eng team" not "Design for 1 billion users." They want to see you make practical trade-offs, not over-engineer.
- **Take-home assignments** — Stripe and Shopify sometimes give 4-8 hour take-home system design docs instead of live whiteboard. You write a design doc with diagrams, trade-offs, and API specs.
- **Production experience valued** — "Tell me about a system you built and what you'd change" is common. They want war stories, not textbook answers.
- **Depth over breadth** — They'd rather you deeply design one component than superficially cover five.

**Company-specific notes:**

| Company | Style | Favorites | Unique Aspect |
|---------|-------|-----------|---------------|
| **Stripe** | Payment systems, API design, idempotency | Payment processing, webhook delivery, ledger system | Obsessed with API design quality. Know idempotency keys, error handling, backward compatibility. |
| **Shopify** | E-commerce at scale, multi-tenant | Storefront rendering, inventory management, checkout | Flash sale handling (10x traffic spikes in seconds). Multi-tenant data isolation. |
| **Datadog** | Observability, time-series data | Metrics ingestion pipeline, alerting system, log aggregation | High-throughput data pipelines. Know time-series DBs, streaming aggregation. |
| **Cloudflare** | Edge computing, CDN, DNS | CDN design, DDoS mitigation, edge workers | Network-level thinking. Know DNS, BGP basics, edge caching strategies. |
| **Coinbase** | Financial systems, blockchain | Trading engine, wallet system, transaction ledger | Double-entry bookkeeping, regulatory compliance, audit trails. |

**Key difference in evaluation:** FAANG asks "can you design at scale?" Remote companies ask "can you design something we'd actually ship?" Start simple, add complexity only when justified.

**Prep adjustment:** If targeting remote companies, spend less time on Tier 3-4 HLD problems and more time on: API design depth (Part 1H), Saga pattern, idempotency, and writing clear design docs.

---

## Part 7: Preparation Schedule

### The 2-Week Sprint (For Working Amazon Professionals)

> **Evidence this works:** An Amazon SDE-2 candidate prepared in exactly 2 weeks and received an offer with 2 Strong Hires + 3 Hires ([GFG interview experience](https://www.geeksforgeeks.org/interview-experiences/amazon-interview-experience-for-a-sde-2/)). A Blind engineer confirmed: "For SDE 2 you don't really need all that much of LC, please do system design and work on communication and stories." The Skilled Coder's Backend Master Sheet states: "Walk through 4–5 LLD problems without drifting into vague class dumping, you are already in strong shape."
>
> **Why this works for YOU:** You already build distributed systems at Amazon daily. You know DynamoDB, SQS, S3, load balancers, auto-scaling. You don't need to learn these — you need to learn how to **talk about them in interview format**.

**Total time: ~30-35 hours over 14 days (~2.5 hrs/weekday, ~4 hrs/weekend day)**

#### Week 1: HLD (System Design)

| Day | Time | What to Do | Specific Items |
|-----|------|-----------|----------------|
| **Day 1** (Mon) | 2.5 hrs | Learn the 4-step framework + back-of-envelope math | Read Part 5 of this guide. Watch [ByteByteGo — Back of Envelope](https://www.youtube.com/watch?v=UC5xf8FbdJc). Practice estimating QPS/storage for 3 random systems. |
| **Day 2** (Tue) | 2.5 hrs | Skim concepts you DON'T know from work | Scan Part 1 concepts #1-76. Skip what you already use at Amazon. Deep-read only: CAP theorem, consistent hashing, fan-out strategies, cache stampede, quorum. |
| **Day 3** (Wed) | 2.5 hrs | HLD Problem 1: URL Shortener | Watch [ByteByteGo video](https://www.youtube.com/watch?v=fMZMm_0ZhK4). Then close it and design it yourself on paper using the 4-step framework. Time yourself to 40 min. |
| **Day 4** (Thu) | 2.5 hrs | HLD Problem 2: Chat System (WhatsApp) | Watch [Gaurav Sen video](https://www.youtube.com/watch?v=vvhC64hQZMk). Redesign yourself. Focus on WebSocket vs polling decision, offline delivery, message ordering. |
| **Day 5** (Fri) | 2.5 hrs | HLD Problem 3: News Feed (Twitter) | Watch [Gaurav Sen video](https://www.youtube.com/watch?v=wYk0xPP_P_8). Key: nail the fan-out-on-write vs fan-out-on-read trade-off. This concept appears in 50% of HLD questions. |
| **Day 6** (Sat) | 4 hrs | HLD Problem 4: Uber + HLD Problem 5: Notification System | Uber: [HelloInterview solution](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber). Notifications: [ByteByteGo video](https://www.youtube.com/watch?v=bBTPZ9NdSk8). Design both yourself after watching. |
| **Day 7** (Sun) | 4 hrs | HLD Problems 6-7: YouTube + Rate Limiter + Mock #1 | YouTube: [ByteByteGo video](https://www.youtube.com/watch?v=jPKTo1iGQiE). Rate Limiter: [ByteByteGo video](https://www.youtube.com/watch?v=FU4WlwfS3G0). Then do 1 mock interview with a friend (pick any problem, 45 min). |

**Week 1 outcome:** You can design 7 systems end-to-end using the framework. You've internalized the pattern: requirements → high-level → deep dive → wrap up.

#### Week 2: LLD + Reinforcement

| Day | Time | What to Do | Specific Items |
|-----|------|-----------|----------------|
| **Day 8** (Mon) | 2.5 hrs | SOLID principles + top 5 design patterns | Read Part 4 of this guide. Focus on: Strategy, Observer, State, Factory, Singleton. Read each on [Refactoring.Guru](https://refactoring.guru/design-patterns). |
| **Day 9** (Tue) | 2.5 hrs | LLD Problem 1: Parking Lot | Read [AlgoMaster solution](https://algomaster.io/learn/lld/design-parking-lot). Model the classes yourself. Key: Strategy pattern for pricing, Factory for vehicle creation. |
| **Day 10** (Wed) | 2.5 hrs | LLD Problem 2: LRU Cache + LLD Problem 3: Elevator | LRU Cache: HashMap + Doubly Linked List (also a coding question — write the code). Elevator: State pattern + Strategy for scheduling. |
| **Day 11** (Thu) | 2.5 hrs | LLD Problem 4: Vending Machine + LLD Problem 5: BookMyShow | Vending Machine: State pattern (NoCoin → HasCoin → Dispensing). BookMyShow: Seat locking with timeout, payment flow. |
| **Day 12** (Fri) | 2.5 hrs | 2 more HLD problems from Tier 2 (pick based on target company) | Amazon → E-Commerce + Distributed Cache. Google → Autocomplete + Web Crawler. Meta → Instagram + Typeahead. |
| **Day 13** (Sat) | 4 hrs | Mock interview #2 + review weak areas | Do a full 45-min mock (HLD). Then do a 40-min mock (LLD). Review: where did you stumble? Which concepts were fuzzy? Re-read those sections. |
| **Day 14** (Sun) | 4 hrs | Mock interview #3 + cheat sheet review | Final mock. Then review: decision matrices (DB, communication, scaling) from Part 8. Review follow-up questions from concept #76. Read company-specific notes for your target. |

**Week 2 outcome:** You can model 5 LLD problems with clean classes and patterns. You've done 3 mock interviews. You know the framework cold.

#### What You're Skipping (And Why It's OK)

| Skipped | Why It's Safe |
|---------|--------------|
| HLD Tier 3 and Tier 4 (15 problems) | These are asked <10% of the time. If you get one, the framework + your 7 practiced problems give you enough pattern to handle it. |
| LLD Tier 2 and Tier 3 (15 problems) | 5 LLD problems with proper patterns covers the structural thinking. Any new problem is just a new domain with the same patterns. |
| Concepts #14-16 (Time-series DB, Graph DB, Blob storage) | You'll mention these naturally when relevant. No one asks "explain time-series databases" as a standalone question. |
| Concepts #29-30 (Consensus, Leader Election) | SDE2 level rarely gets grilled on Raft/Paxos internals. Mention "we'd use ZooKeeper for leader election" and move on. |
| 10 of the 14 design patterns | Strategy, Observer, State, Factory, Singleton cover 90% of LLD interviews. The rest are nice-to-know. |

#### The Math: Why 2 Weeks Works for an Amazon SDE

| What You Already Know From Work | What You Actually Need to Learn |
|--------------------------------|-------------------------------|
| Load balancers, auto-scaling | The 4-step interview framework |
| DynamoDB, S3, SQS, SNS | How to articulate trade-offs out loud |
| Microservices, API design | Back-of-envelope estimation |
| Monitoring, alarms, dashboards | Fan-out strategies, consistent hashing |
| Code reviews, OOP, clean code | LLD interview format (class modeling on whiteboard) |

You're not learning system design from scratch. You're learning **how to perform system design in a 45-minute interview format**. That's a much smaller gap.

---

### 4-Week Comfortable Plan

| Week | Focus |
|------|-------|
| 1 | All 54 concepts (skim what you know, deep-read what you don't) + HLD Tier 1 (1-5) |
| 2 | HLD Tier 1 (6-10) + LLD Tier 1 (1-5) |
| 3 | HLD Tier 2 (top 5) + LLD Tier 1 (6-10) + Design Patterns |
| 4 | Mock interviews + review weak areas |

### 6-Week Plan for SDE1 → SDE2 Transition

> **Who this is for:** SDE1 at a startup or non-FAANG company who hasn't used DynamoDB, Kafka, or Redis at work. You need to LEARN concepts before practicing problems.

| Week | Focus | Hours | What Exactly |
|------|-------|-------|-------------|
| **1** | Foundations Part 1 | ~10 hrs | Scaling (#1-6), Data Storage (#7-14), ACID/BASE, CAP theorem. Watch one ByteByteGo video per concept. Do back-of-envelope exercises for 5 random apps. |
| **2** | Foundations Part 2 | ~10 hrs | Caching (#19-25), Communication (#24-31), Message Queues, Pub/Sub. Set up a local Redis and play with it for 1 hour — seeing it work makes it stick. |
| **3** | Distributed Systems + Reliability | ~10 hrs | Concepts #32-50: Consistency, Replication, Circuit Breaker, Sagas, CDC, SLIs/SLOs. Read DDIA chapters 5-9 if you have the book. |
| **4** | HLD Problems (Easy → Medium) | ~12 hrs | URL Shortener, Rate Limiter, Notification System (🟢 Easy). Then Chat System, News Feed (🟡 Medium). Use the 4-step framework for each. Time yourself to 40 min. |
| **5** | LLD + Design Patterns | ~12 hrs | SOLID principles + Strategy/Observer/State/Factory/Singleton patterns. Then: Parking Lot, LRU Cache, Elevator, Vending Machine. Model classes on paper before coding. |
| **6** | Mocks + Hard Problems + Company Prep | ~12 hrs | 3 mock interviews (1 HLD, 1 LLD, 1 mixed). YouTube/Uber (🔴 Hard). Review company-specific notes. Fill weak areas from mock feedback. |

**Key difference from the 2-week plan:** Weeks 1-3 are concept-only (no problems). This builds the vocabulary you need so that when you hit problems in week 4, you're not Googling "what is consistent hashing" mid-design.

**How to know you're ready for problems:** Can you explain CAP theorem, cache-aside, fan-out-on-write, and Saga pattern to a friend without looking at notes? If yes → start problems. If no → re-read those concepts.

### Why No 8-Week Plan?

If you have 8 weeks, do the 4-week plan thoroughly + spend weeks 5-8 on mock interviews and weak-area reinforcement. Spreading content over 8 weeks creates a false sense of progress. Intensity beats duration for interview prep. The 4-week plan covers everything you need — extra time should go to practice, not more content consumption.

---

## Part 8: Master Resource List

### Books (Pick ONE, Read Cover to Cover)
1. **System Design Interview Vol 1** — Alex Xu → Best for HLD problems
2. **System Design Interview Vol 2** — Alex Xu → Advanced problems
3. **Designing Data-Intensive Applications** — Martin Kleppmann → Deep understanding of distributed systems

### YouTube Channels (Free)
| Channel | Best For | Link |
|---------|----------|------|
| ByteByteGo | HLD concepts + problems (visual) | https://www.youtube.com/@ByteByteGo |
| Gaurav Sen | HLD concepts + problems (intuitive) | https://www.youtube.com/@gaborsen |
| Jordan Has No Life | Deep distributed systems theory | https://www.youtube.com/@jordanhasnolife5163 |
| sudoCODE | HLD problems (detailed) | https://www.youtube.com/@sudocode |
| codeKarle | HLD problems (practical) | https://www.youtube.com/@codeKarle |
| Tech Dummies | HLD problems | https://www.youtube.com/@TechDummiesNarendraL |
| Concept && Coding | LLD problems (Java) | https://www.youtube.com/@ConceptandCoding |

### Websites (Free + Paid)
| Site | Best For | Link |
|------|----------|------|
| HelloInterview | HLD problem breakdowns (free) | https://www.hellointerview.com/learn/system-design |
| SystemDesignSchool | HLD solutions (free) | https://systemdesignschool.io |
| System Design Handbook | Structured guides + company-specific prep | https://www.systemdesignhandbook.com |
| DesignGurus.io | HLD + LLD courses (paid) | https://www.designgurus.io |
| AlgoMaster | LLD problems (free) | https://algomaster.io/learn/lld |
| Refactoring.Guru | Design patterns (free) | https://refactoring.guru/design-patterns |
| Codemia.io | Practice SD problems (free) | https://www.codemia.io |
| ByteByteGo (newsletter) | Weekly SD concepts | https://blog.bytebytego.com |
| System Design Primer (GitHub) | Comprehensive reference | https://github.com/donnemartin/system-design-primer |
| The Skilled Coder | Backend master prep sheet | https://newsletter.theskilledcoder.com |

### Practice Platforms
| Platform | What It Offers |
|----------|---------------|
| HelloInterview | Free HLD problem breakdowns with solutions |
| Codemia.io | Practice SD problems, evaluate your solutions |
| Excalidraw | Free whiteboarding tool for practice | https://excalidraw.com |
| IGotAnOffer | Paid mock interviews with ex-FAANG interviewers |

---

### 📖 Glossary (Quick Reference for Unfamiliar Terms)

| Term | Definition |
|------|-----------|
| **Backpressure** | Mechanism to slow down producers when consumers can't keep up. Example: bounded queue blocks producers when full. |
| **Bloom Filter** | Probabilistic data structure: "definitely not in set" or "probably in set." Used to avoid unnecessary DB lookups. |
| **CAP** | Consistency, Availability, Partition tolerance — pick CP or AP during network partitions. |
| **CDC** | Change Data Capture — streaming row-level DB changes (INSERT/UPDATE/DELETE) as events via the transaction log. |
| **Compensating Transaction** | An undo operation in a Saga. If step 3 fails, step 2's compensating transaction reverses its effect. |
| **Consistent Hashing** | Hashing technique where adding/removing servers only remaps ~1/N keys instead of all keys. |
| **CQRS** | Command Query Responsibility Segregation — separate read and write models for different optimization. |
| **Denormalization** | Duplicating data across tables/documents to speed up reads at the cost of write complexity. |
| **Fan-out** | Distributing data to multiple recipients. Fan-out-on-write = push to followers' feeds. Fan-out-on-read = compute feed on request. |
| **Geohash** | Encoding latitude/longitude into a string. Nearby locations share prefixes. Used for proximity queries. |
| **Gossip Protocol** | Peer-to-peer protocol where nodes exchange state with random peers, eventually converging. Used in Cassandra. |
| **HPA** | Horizontal Pod Autoscaler — Kubernetes component that auto-scales pods based on metrics. |
| **Idempotency** | Operation that produces the same result regardless of how many times it's executed. Critical for payment APIs. |
| **Jitter** | Random delay added to retries to prevent thundering herd (all clients retrying at the same time). |
| **Load Shedding** | Intentionally dropping low-priority requests during overload to protect high-priority ones. |
| **LSM Tree** | Log-Structured Merge Tree — write-optimized data structure used by Cassandra, RocksDB. |
| **Outbox Pattern** | Write event to an "outbox" table in the same DB transaction as business data, then publish to Kafka via CDC. |
| **Partition** | (1) Network partition: nodes can't communicate. (2) Data partition: splitting data across servers (sharding). |
| **Quorum** | Majority agreement in a replicated system. W + R > N = strong consistency. |
| **Saga** | Sequence of local transactions across services with compensating transactions for rollback. |
| **Shard** | A horizontal partition of a database. Each shard holds a subset of the data. |
| **SLI/SLO/SLA** | Service Level Indicator (metric) / Objective (target) / Agreement (contract with consequences). |
| **Thundering Herd** | Many clients simultaneously hitting a resource (e.g., cache miss causing DB flood). |
| **Tombstone** | A marker indicating deleted data in eventually consistent systems. Actual deletion happens during compaction. |
| **Vnode** | Virtual node — multiple hash ring positions per physical server for even distribution in consistent hashing. |
| **WAL** | Write-Ahead Log — append-only log written before data modification for crash recovery. |

---

## Quick Reference: Cheat Sheet

### Problem → Key Concepts Map (Study Guide)

> Use this to know which concepts to review before practicing each problem.

| Problem | Key Concepts You Need |
|---------|----------------------|
| **HLD-1. URL Shortener** | #6 (estimation), #7 (SQL vs NoSQL), #14 (consistent hashing), #19 (caching), #35 (idempotency) |
| **HLD-2. Rate Limiter** | #4 (rate limiting algorithms), #5 (API gateway), #21 (Redis) |
| **HLD-3. WhatsApp** | #29 (WebSockets), #30 (WS at scale), #31 (message queues), #39 (eventual consistency), #18 (data modeling) |
| **HLD-4. News Feed** | #19 (caching), #32 (pub/sub), #21 (Redis sorted sets), #8 (sharding) |
| **HLD-5. YouTube** | #17 (blob storage), #22 (CDN), #31 (message queues), #6 (estimation) |
| **HLD-6. Dropbox** | #17 (blob storage), #29 (WebSockets), #38 (idempotency), #9 (replication) |
| **HLD-7. Uber** | #10 (geospatial indexing), #29 (WebSockets), #31 (message queues), #8 (sharding) |
| **HLD-8. Notifications** | #31 (message queues), #32 (pub/sub), #38 (idempotency), #4 (rate limiting) |
| **HLD-9. Web Crawler** | #45 (bloom filters), #31 (message queues), #17 (blob storage) |
| **HLD-10. Autocomplete** | #19 (caching), #6 (estimation), #10 (indexing) |
| **LLD-1. Parking Lot** | Strategy pattern, Factory, Singleton, SOLID |
| **LLD-2. Elevator** | State pattern, Strategy pattern, Observer |
| **LLD-5. LRU Cache** | HashMap + Doubly Linked List, #66 (thread safety for follow-up) |
| **LLD-6. Vending Machine** | State pattern |
| **LLD-8. Splitwise** | Strategy pattern, Observer, graph algorithms |

---

### Database Decision Matrix
```
Need ACID transactions?          → SQL (PostgreSQL, MySQL)
Need flexible schema?            → Document DB (MongoDB, DynamoDB)
Need high write throughput?      → Column DB (Cassandra)
Need relationship traversal?     → Graph DB (Neo4j)
Need full-text search?           → Search Engine (Elasticsearch)
Need time-series data?           → Time-series DB (InfluxDB, Timestream)
Need simple key-value cache?     → Redis / Memcached
Need file/blob storage?          → Object Store (S3)
```

### Communication Decision Matrix
```
Client ↔ Server (request-response)?  → REST / gRPC
Server → Client (real-time push)?    → WebSocket / SSE
Service → Service (async)?           → Message Queue (Kafka, SQS)
Service → Multiple Services (event)? → Pub/Sub (Kafka, SNS)
```

### Scaling Decision Matrix
```
Stateless service?     → Horizontal scaling + Load Balancer
Database reads slow?   → Add read replicas + caching (Redis)
Database writes slow?  → Sharding + write-behind cache
Global users?          → Multi-region + CDN
Spiky traffic?         → Auto-scaling + message queue to absorb bursts
Hot partition?         → Better partition key + consistent hashing
```

---

> **Total items:** 76 concepts + 37 HLD problems + 25 LLD problems + 14 design patterns + SOLID principles + trade-off framework + worked examples = **155+ unique topics**
>
> **Every item sourced from real FAANG interview data (Glassdoor, Blind, onsites.fyi, interviewing.io, candidate experiences).**
>
> **This guide covers ~95% of what's asked at SDE2 level. For the remaining ~5% (novel problems), the 4-step framework, trade-off discussion skills, and foundational concepts transfer directly. The framework matters more than the problem list.**
