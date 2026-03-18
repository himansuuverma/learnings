# SDE2 System Design Interview — The Only Guide You Need

> **Target:** SDE2 (Amazon L5) / Google L4 / Meta E4 at Amazon, Google, Meta, Microsoft, Apple, Uber, Netflix, Anthropic, OpenAI
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
- #45-47 (SPOF, Failover, Health Checks)

**Read these (common blind spots for working SDEs):**
- #6 (Back-of-Envelope Math) — you've never done this in an interview format
- #12 (Isolation Levels) — if you mostly use NoSQL, this is a gap
- #13-14 (CAP Theorem, Consistent Hashing) — you know the result, not the theory
- #18 (Data Modeling) — can you model for BOTH SQL and NoSQL under time pressure?
- #27 (WebSocket Management at Scale) — deep dive most SDEs never need at work
- #60 (Thread Safety) — rusty if your services are serverless or single-threaded

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
- [Part 1: Foundational Concepts (69 Topics)](#part-1-foundational-concepts)
  - [1A. Scaling & Traffic Management](#1a-scaling--traffic-management)
  - [1B. Data Storage & Data Modeling](#1b-data-storage--data-modeling)
  - [1C. Caching](#1c-caching)
  - [1D. Communication & Networking](#1d-communication--networking)
  - [1E. Distributed Systems Concepts](#1e-distributed-systems-concepts)
  - [1F. Reliability & Observability](#1f-reliability--observability)
  - [1G. Security Basics](#1g-security-basics)
  - [1H. API Design Fundamentals](#1h-api-design-fundamentals)
  - [1I. Cost Awareness](#1i-cost-awareness)
  - [1J. Concurrency & Thread Safety](#1j-concurrency--thread-safety)
- [Part 2: HLD Problems — "Design X" (37 Problems)](#part-2-hld-problems)
  - [Fully Worked Example: URL Shortener](#fully-worked-example-url-shortener)
- [Part 3: LLD Problems — Object-Oriented Design (25 Problems)](#part-3-lld-problems)
  - [UML Basics for Interviews](#uml-basics-for-interviews)
  - [Fully Worked: Parking Lot](#fully-worked-parking-lot)
  - [Fully Worked: LRU Cache](#fully-worked-lru-cache)
  - [Fully Worked: Elevator System](#fully-worked-elevator-system)
- [Part 4: Design Patterns for LLD](#part-4-design-patterns)
- [Part 5: The Interview Framework + Trade-off Mastery](#part-5-interview-framework)
- [Part 6: Company-Specific Notes](#part-6-company-specific-notes)
- [Part 7: Preparation Schedule](#part-7-preparation-schedule)
- [Part 8: Master Resource List](#part-8-master-resource-list)

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

## Part 1: Foundational Concepts (69 Topics)

> You cannot design anything without these. For each concept: understand what it is, when to use it, trade-offs, and be able to explain it in 2 minutes during an interview.

---

### 1A. Scaling & Traffic Management

#### 1. Horizontal vs Vertical Scaling

**What:** Vertical = bigger machine (more CPU/RAM). Horizontal = more machines.

**When to use what:**
- Vertical: Simple, works for small-medium scale. Has a hard ceiling.
- Horizontal: Required for large-scale systems. Adds complexity (data consistency, load balancing).

**Interview talking point:** "Vertical scaling has a hardware ceiling and creates a single point of failure. For any system serving millions of users, horizontal scaling is the default — we add stateless servers behind a load balancer."

**Trade-off:** Vertical is simpler but limited. Horizontal is scalable but requires stateless design, distributed data, and load balancing.

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

📺 [ByteByteGo — Rate Limiter](https://www.youtube.com/watch?v=FU4WlwfS3G0)
📖 [HelloInterview — Rate Limiter Design](https://www.hellointerview.com/learn/system-design/problem-breakdowns/rate-limiter)

---

#### 5. API Gateway

**What:** Single entry point for all client requests. Handles routing, authentication, rate limiting, request transformation, and monitoring.

**Responsibilities:** Auth, rate limiting, request routing, protocol translation, response caching, logging.

**Interview talking point:** "All client requests hit our API Gateway first. It handles JWT validation, rate limiting, and routes to the appropriate microservice. This decouples clients from internal service topology."

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

---

#### 11. ACID vs BASE

| ACID (SQL) | BASE (NoSQL) |
|------------|-------------|
| **A**tomicity — all or nothing | **B**asically **A**vailable — system always responds |
| **C**onsistency — valid state transitions | **S**oft state — state may change over time |
| **I**solation — concurrent txns don't interfere | **E**ventual consistency — will converge eventually |
| **D**urability — committed data survives crashes | |

**When to mention:** Payment systems need ACID. Social media feeds can use BASE.

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

📺 [ByteByteGo — CAP Theorem](https://www.youtube.com/watch?v=_RbsFXWRZ10)
📖 [HelloInterview — CAP Theorem](https://www.hellointerview.com/learn/system-design/deep-dives/cap-theorem)

---

#### 14. Consistent Hashing

**What:** A hashing technique where adding/removing servers only requires remapping `K/N` keys (K = total keys, N = total servers), instead of rehashing everything.

**How:** Servers and keys are placed on a virtual ring. Each key is assigned to the next server clockwise on the ring. Virtual nodes (vnodes) ensure even distribution.

**Used in:** DynamoDB, Cassandra, load balancers, distributed caches.

**Interview talking point:** "For our distributed cache, I'd use consistent hashing with 150 virtual nodes per server. When we add a new cache server, only ~1/N of keys need to be remapped."

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

📺 [ByteByteGo — Caching Strategies](https://www.youtube.com/watch?v=ccemOqDrc2I)
📖 [IGotAnOffer — Caching](https://igotanoffer.com/blogs/tech/caching-system-design-interview)

---

#### 20. Cache Eviction Policies

- **LRU (Least Recently Used):** Evict the item not accessed for the longest time. Most common.
- **LFU (Least Frequently Used):** Evict the item accessed the fewest times.
- **TTL (Time-To-Live):** Items expire after a set duration.
- **FIFO:** First in, first out.

**Interview default:** "LRU with a TTL of 5-15 minutes covers most cases."

---

#### 21. Redis vs Memcached

| | Redis | Memcached |
|---|---|---|
| Data structures | Strings, lists, sets, sorted sets, hashes | Strings only |
| Persistence | Optional (RDB/AOF) | None |
| Replication | Built-in | None |
| Use case | Cache + data store, leaderboards, sessions, pub/sub | Pure caching, simple key-value |

**Interview default:** Use Redis unless you have a specific reason for Memcached.

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

### 1D. Communication & Networking

#### 24. REST vs gRPC vs GraphQL

| | REST | gRPC | GraphQL |
|---|---|---|---|
| Protocol | HTTP/1.1 | HTTP/2 | HTTP |
| Format | JSON | Protobuf (binary) | JSON |
| Speed | Moderate | Fast (binary, multiplexed) | Moderate |
| Best for | Public APIs, CRUD | Internal microservice-to-microservice | Mobile apps, flexible queries |
| Streaming | No (needs WebSocket) | Yes (bidirectional) | Subscriptions |

**Interview default:** REST for external APIs, gRPC for internal service communication.

---

#### 25. Sync vs Async Communication

- **Synchronous:** Caller waits for response. HTTP request-response. Simple but creates coupling.
- **Asynchronous:** Caller sends message and moves on. Uses message queues. Decoupled, resilient.

**When to use async:** Email sending, video processing, notification delivery, any non-time-critical operation.

---

#### 26. WebSockets vs Long Polling vs SSE

| | WebSocket | Long Polling | SSE (Server-Sent Events) |
|---|---|---|---|
| Direction | Bidirectional | Client-initiated | Server → Client only |
| Connection | Persistent | Repeated HTTP | Persistent HTTP |
| Use case | Chat, gaming, live collab | Fallback for WebSocket | Live feeds, notifications |
| Overhead | Low after handshake | High (repeated connections) | Low |

**Interview talking point:** "For real-time chat, I'd use WebSockets for bidirectional communication. For a live news feed where the server pushes updates, SSE is simpler and sufficient."

📺 [IGotAnOffer — Polling, SSE, WebSockets](https://igotanoffer.com/blogs/tech/polling-sse-websockets-system-design-interview)

---

#### 27. WebSocket Connection Management at Scale

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

#### 28. Message Queues

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

📺 [ByteByteGo — Message Queues](https://www.youtube.com/watch?v=W4_aGb_MOls)
📖 [IGotAnOffer — Queues & Pub/Sub](https://igotanoffer.com/blogs/tech/queues-pubsub-system-design-interview)

---

#### 29. Pub/Sub Pattern

**What:** Publishers emit events to topics. Subscribers listen to topics they care about. Publishers don't know about subscribers.

**Difference from Queue:** In a queue, each message is consumed by ONE consumer. In pub/sub, each message goes to ALL subscribers.

**Examples:** Kafka topics, Google Pub/Sub, Amazon SNS, Redis Pub/Sub.

---

#### 30. Event-Driven Architecture

**What:** Services communicate by producing and consuming events rather than direct API calls.

**Benefits:** Loose coupling, independent scaling, better fault isolation, natural audit trail.

**Pattern:** Service A emits "OrderCreated" event → Kafka → Service B (inventory), Service C (notification), Service D (analytics) all consume independently.

---

#### 31. Service Discovery

**What:** How services find each other in a microservices architecture where instances are dynamic.

**Approaches:**
- **Client-side discovery:** Client queries a registry (e.g., Eureka) and picks an instance.
- **Server-side discovery:** Load balancer queries registry and routes. Client just hits the LB.
- **DNS-based:** Services register DNS entries. Simple but slow to update.

---

### 1E. Distributed Systems Concepts

#### 32. Consensus Algorithms (Raft, Paxos)

> 🔶 **STRETCH** — SDE2 is never asked to explain Raft/Paxos internals. Just know: "We'd use ZooKeeper or etcd for leader election and distributed coordination."

**What:** Protocols that allow distributed nodes to agree on a single value even if some nodes fail. Raft uses leader election + log replication. Majority (quorum) must agree.

**When to mention:** Leader election, distributed config stores. One sentence: "Leader election is handled by ZooKeeper using a consensus protocol."

---

#### 33. Leader Election

**What:** Process of designating one node as the coordinator (leader) among a group of nodes.

**Used in:** Database replication (who accepts writes), distributed locks, task scheduling.

**Tools:** ZooKeeper, etcd, Consul.

📖 [IGotAnOffer — Leader Election](https://igotanoffer.com/blogs/tech/leader-election-system-design-interview)

---

#### 34. Distributed Locking

**What:** Ensuring only one process across multiple servers can access a shared resource at a time.

**Implementations:** Redis (Redlock algorithm), ZooKeeper, DynamoDB conditional writes.

**Interview talking point:** "To prevent double-booking a ticket, I'd use a distributed lock via Redis with a TTL. The service acquires the lock on `seat_id`, processes the booking, then releases it."

---

#### 35. Idempotency

**What:** An operation that produces the same result regardless of how many times it's executed.

**Why it matters:** Network retries can cause duplicate requests. Without idempotency, you might charge a user twice.

**Implementation:** Client sends a unique `idempotency_key` with each request. Server checks if this key was already processed before executing.

**Interview talking point:** "All our payment APIs are idempotent. The client generates a UUID as an idempotency key. If we receive a duplicate, we return the cached response."

---

#### 36. Eventual vs Strong Consistency

- **Strong:** After a write, all subsequent reads return the updated value. Higher latency.
- **Eventual:** After a write, reads may return stale data for a short period. Lower latency, higher availability.

**Decision framework:**
- User profile updates → eventual consistency is fine
- Bank balance → strong consistency required
- Social media feed → eventual consistency is fine
- Inventory count during checkout → strong consistency required

---

#### 37. Data Partitioning Strategies

Same as sharding (covered in #8), but also applies to:
- **Kafka topics:** Partition by key for ordering guarantees
- **Cache clusters:** Partition by consistent hashing
- **Search indices:** Partition by document ID or time range

---

#### 38. Replication Strategies

- **Synchronous:** Leader waits for replica ACK before confirming write. Strong consistency, higher latency.
- **Asynchronous:** Leader confirms write immediately, replicates in background. Lower latency, risk of data loss.
- **Semi-synchronous:** Wait for at least 1 replica ACK. Balance of both.

---

#### 39. Conflict Resolution

**When:** Multi-leader or leaderless replication where concurrent writes to same key happen.

**SDE2 level — know these two:**
- **Last-Write-Wins (LWW):** Timestamp-based. Simple but can lose data. **Default answer for SDE2.**
- **Application-level:** Let the user resolve (e.g., Google Docs shows both versions).

> 🔶 **STRETCH:** Vector Clocks (track causal ordering) and CRDTs (auto-merging data structures) are SDE3 territory. At SDE2, just say "LWW with timestamp" and move on.

---

#### 40. Circuit Breaker Pattern

**What:** Prevents a service from repeatedly calling a failing downstream service. After N failures, the circuit "opens" and requests fail fast without calling the downstream.

**States:** Closed (normal) → Open (failing fast) → Half-Open (testing recovery).

**Interview talking point:** "If our payment service is down, the circuit breaker opens after 5 consecutive failures. For 30 seconds, all payment requests fail fast with a friendly error. Then it tries one request — if it succeeds, the circuit closes."

---

#### 41. Retry with Exponential Backoff

**What:** On failure, retry with increasing delays: 1s, 2s, 4s, 8s... plus random jitter to prevent thundering herd.

**Formula:** `delay = min(base * 2^attempt + random_jitter, max_delay)`

**Interview talking point:** "Failed API calls are retried with exponential backoff starting at 100ms, capped at 30 seconds, with random jitter to avoid synchronized retries."

---

#### 42. Bloom Filters

**What:** Space-efficient probabilistic data structure. Tells you "definitely not in set" or "probably in set." False positives possible, false negatives impossible.

**SDE2 level:** Know what it does and when to mention it. Don't memorize the math.

**When to mention:** "Before querying the DB, we check a Bloom filter to eliminate unnecessary lookups." That's the full answer at SDE2.

---

#### 43. Multi-Region Architecture

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

#### 44. Merkle Trees *(🔶 STRETCH — rarely asked at SDE2)*

**What:** Hash tree to verify data consistency between replicas. Comparing root hashes tells you instantly if replicas are in sync. Used in DynamoDB, Git, Cassandra. Just know it exists — don't prep this.

---

### 1F. Reliability & Observability

#### 45. Single Point of Failure (SPOF)

**What:** Any component whose failure brings down the entire system.

**How to eliminate:** Redundancy at every layer — multiple app servers, DB replicas, multi-AZ deployment, redundant load balancers.

**Interview talking point:** "I'd deploy across 3 availability zones. Each component — load balancer, app servers, database — has redundant instances. No single failure takes down the system."

---

#### 46. Failover Mechanisms

- **Active-Passive:** Standby takes over when primary fails. Simple, some downtime during switchover.
- **Active-Active:** Both handle traffic. Zero downtime. More complex (data sync needed).

---

#### 47. Health Checks & Heartbeats

- **Health checks:** Load balancer periodically pings servers. Unhealthy servers are removed from rotation.
- **Heartbeats:** Nodes send periodic signals to a coordinator. Missing heartbeats trigger failover.

---

#### 48. Monitoring, Logging, Alerting

**The three pillars of observability:**
1. **Metrics:** Numeric measurements over time (QPS, latency p99, error rate, CPU). Tools: Prometheus, CloudWatch, Datadog.
2. **Logs:** Detailed event records. Tools: ELK stack, Splunk, CloudWatch Logs.
3. **Traces:** Request flow across services. Tools: Jaeger, X-Ray, Zipkin.

**Interview talking point:** "I'd monitor p99 latency, error rate, and QPS with dashboards and alerts. If p99 > 500ms or error rate > 1%, we get paged."

---

#### 49. Distributed Tracing

**What:** Tracking a single request as it flows through multiple microservices. Each service adds a span to the trace.

**How:** A unique trace ID is generated at the entry point and propagated through all downstream calls via headers.

---

#### 50. SLIs, SLOs, SLAs & Error Budgets

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

### 1G. Security Basics

#### 51. Authentication vs Authorization

- **Authentication (AuthN):** Who are you? (Login, JWT, OAuth)
- **Authorization (AuthZ):** What can you do? (Roles, permissions, RBAC)

**Common patterns:** JWT tokens for stateless auth, OAuth 2.0 for third-party access, API keys for service-to-service.

---

#### 52. Encryption

- **At rest:** Data encrypted in storage (AES-256). Protects against physical theft.
- **In transit:** Data encrypted during transmission (TLS/HTTPS). Protects against eavesdropping.
- **End-to-end:** Only sender and receiver can decrypt (e.g., WhatsApp). Server cannot read content.

---

#### 53. DDoS Protection

**What:** Distributed Denial of Service — overwhelming a service with traffic.

**Mitigations:** Rate limiting, CDN (absorbs traffic), WAF (Web Application Firewall), auto-scaling, IP blacklisting.

---

### 1H. API Design Fundamentals

> This section is tested in 60%+ of SDE2 interviews. Meta's "Product Architecture" round is essentially API design. Google drills into API contracts. Amazon's LLD round often starts with "define the APIs first."

#### 54. RESTful API Design

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

#### 55. Pagination

| Strategy | How It Works | Pros | Cons |
|----------|-------------|------|------|
| **Offset-based** | `?offset=20&limit=10` | Simple, supports "jump to page" | Slow on large datasets (DB scans), inconsistent with inserts |
| **Cursor-based** | `?cursor=eyJpZCI6MTIzfQ&limit=10` | Fast (seeks to cursor), consistent | No "jump to page N", opaque cursor |
| **Keyset** | `?after_id=123&limit=10` | Fast, simple | Requires sortable unique key |

**Interview default:** "For feeds and timelines, I'd use cursor-based pagination — it's consistent even when new items are inserted, and it's O(1) seek time with an index. The cursor is a base64-encoded `{last_seen_id, last_seen_timestamp}`."

#### 56. API Versioning

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| **URL path** | `/v1/users`, `/v2/users` | Explicit, easy to route | URL pollution |
| **Header** | `Accept: application/vnd.api+json;version=2` | Clean URLs | Hidden, harder to test |
| **Query param** | `/users?version=2` | Easy to test | Clutters params |

**Interview default:** URL path versioning (`/v1/`) — it's the most common and easiest to reason about in a design discussion.

#### 57. Error Response Design

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

#### 58. Idempotency in APIs

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

#### 59. Cost-Aware Design Decisions

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

#### 60. Thread Safety Fundamentals

**What:** Ensuring that shared data is accessed correctly when multiple threads execute concurrently.

**Core primitives:**

| Primitive | What It Does | When to Use |
|-----------|-------------|-------------|
| **Mutex/Lock** | Only one thread can hold it at a time | Protecting critical sections (e.g., updating a shared counter) |
| **Read-Write Lock** | Multiple readers OR one writer | Read-heavy data (e.g., cache — many reads, rare writes) |
| **Semaphore** | Allows N concurrent accesses | Connection pools, rate limiting (e.g., max 10 DB connections) |
| **CAS (Compare-And-Swap)** | Atomic update: "set to Y only if currently X" | Lock-free counters, optimistic concurrency |

**Common thread-safe patterns:**

```python
# 1. Lock-based: Simple but can cause contention
import threading

class ThreadSafeCounter:
    def __init__(self):
        self._count = 0
        self._lock = threading.Lock()

    def increment(self):
        with self._lock:
            self._count += 1
            return self._count

# 2. Read-Write Lock: Better for read-heavy workloads
class ReadWriteCache:
    def __init__(self):
        self._data = {}
        self._lock = threading.RLock()  # Reentrant for nested reads
        self._readers = 0

    def get(self, key):
        with self._lock:
            return self._data.get(key)

    def put(self, key, value):
        with self._lock:
            self._data[key] = value
```

**Producer-Consumer pattern (asked frequently):**
```python
import queue
import threading

class TaskProcessor:
    def __init__(self, num_workers: int = 4):
        self._queue = queue.Queue(maxsize=100)  # bounded queue — blocks when full
        self._workers = []
        for _ in range(num_workers):
            t = threading.Thread(target=self._worker, daemon=True)
            t.start()
            self._workers.append(t)

    def submit(self, task):
        self._queue.put(task)  # blocks if queue full (backpressure)

    def _worker(self):
        while True:
            task = self._queue.get()  # blocks if queue empty
            try:
                task.execute()
            finally:
                self._queue.task_done()
```

**Common interview follow-ups:**
- "Make your LRU Cache thread-safe" → Wrap `get`/`put` in a lock. For better performance, use a read-write lock (reads don't block each other).
- "Design a thread-safe singleton" → Double-checked locking or use language-level guarantees (Python module-level, Java `enum`).
- "How do you avoid deadlocks?" → Always acquire locks in the same order. Use timeouts on lock acquisition. Prefer lock-free structures when possible.

**Interview talking point:** "I'd use a read-write lock for the cache since our read:write ratio is 100:1. Multiple threads can read concurrently, and only writes acquire an exclusive lock. For the task queue, I'd use a bounded blocking queue with a thread pool — this gives us natural backpressure when the system is overloaded."

---

#### 61. Kubernetes / Container Orchestration Basics

**What:** Container orchestration platform for deploying, scaling, and managing containerized applications. At SDE2 level, you don't need deep K8s expertise, but you should know the vocabulary.

**Key concepts:**
| Concept | What It Is | Interview Equivalent |
|---------|-----------|---------------------|
| **Pod** | Smallest deployable unit (1+ containers) | "An instance of our service" |
| **Deployment** | Manages pod replicas, rolling updates | "Auto-scaling group" |
| **Service** | Stable network endpoint for pods | "Internal load balancer" |
| **HPA** (Horizontal Pod Autoscaler) | Auto-scales pods based on metrics | "Auto-scaling policy" |
| **ConfigMap / Secret** | External configuration injection | "Environment variables / secrets manager" |
| **Namespace** | Logical isolation within a cluster | "Separate environments (dev/staging/prod)" |

**Interview talking point:** "I'd deploy this as a Kubernetes Deployment with 3 replicas and an HPA targeting 60% CPU utilization. The service is exposed via a Kubernetes Service with an ingress controller for external traffic. Config is injected via ConfigMaps, secrets via K8s Secrets backed by a vault."

---

#### 62. Service Mesh *(Know the concept — mention when discussing microservice communication)*

**What:** Infrastructure layer that handles service-to-service communication. A sidecar proxy (e.g., Envoy) is deployed alongside each service instance, handling traffic management, security (mTLS), and observability transparently.

**Examples:** Istio, Linkerd, AWS App Mesh.

**When to mention:** When your design has 5+ microservices and the interviewer asks about service-to-service security, traffic routing, or observability. Say: "For inter-service communication, I'd use a service mesh like Istio. Each service gets an Envoy sidecar that handles mTLS encryption, retry policies, circuit breaking, and distributed tracing — without changing application code."

---

#### 63. Capacity Planning

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

#### 64. Cache Stampede / Thundering Herd

**What:** When a popular cache key expires, hundreds of concurrent requests simultaneously hit the database to regenerate it, potentially crashing the DB.

**Solutions:**
- **Locking:** First request acquires a lock and rebuilds cache. Others wait or get stale data.
- **Early expiration:** Refresh cache before TTL expires (background refresh).
- **Staggered TTL:** Add random jitter to TTL so keys don't expire simultaneously.

**Interview talking point:** "For hot keys, I'd use a locking mechanism — the first request on cache miss acquires a distributed lock, rebuilds the cache, and all other requests wait briefly or serve stale data. This prevents thundering herd on our database."

---

#### 65. Microservices vs Monolith

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

#### 66. Gossip Protocol *(🔶 STRETCH — just know the name)*

**What:** Peer-to-peer protocol where nodes exchange state with random peers, eventually converging. Used in Cassandra for failure detection. One sentence: "Cassandra uses gossip protocol for node failure detection."

---

#### 67. Write-Ahead Log (WAL)

**What:** Before any data modification, the change is first written to an append-only log. If the system crashes, replay the log to recover. Used in PostgreSQL, MySQL, Kafka.

**SDE2 level:** One sentence when discussing durability: "The database uses a write-ahead log for crash recovery."

---

#### 68. Quorum Reads/Writes

**What:** In a replicated system with N replicas, a quorum requires agreement from a majority.

**SDE2 level:** Know the rule: **W + R > N = strong consistency.** Common config: N=3, W=2, R=2.

**Interview talking point:** "With 3 replicas, I'd set W=2, R=2 for strong consistency while tolerating one node failure."

> 🔶 **STRETCH:** Detailed tuning of W/R values for different consistency levels is SDE3. At SDE2, the one-liner above is sufficient.

---

#### 69. Common Follow-Up Questions (Prepare for These)

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

---

### ⭐ Fully Worked Example: URL Shortener (End-to-End Using the 4-Step Framework)

> This is exactly how you should present ANY HLD problem. Study this flow, then apply it to every other problem.

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

#### HLD-1. Design a URL Shortener (TinyURL)

**Requirements:** Generate short URL from long URL, redirect short → long, analytics (optional), custom aliases (optional).

**Key decisions:**
- **ID generation:** Base62 encoding of auto-increment ID, or MD5/SHA hash truncated to 7 chars
- **Storage:** Key-value store (DynamoDB) — key: short_code, value: long_url
- **Read:Write ratio:** ~100:1 → cache heavily
- **Cache:** Redis for hot URLs
- **Scale:** Consistent hashing for DB sharding by short_code

**Back-of-envelope:** 100M URLs/day = ~1200 writes/sec. 10B redirects/day = ~120K reads/sec.

📺 [ByteByteGo — URL Shortener](https://www.youtube.com/watch?v=fMZMm_0ZhK4)
📺 [Gaurav Sen — URL Shortener](https://www.youtube.com/watch?v=JQDHz72OA3c)
📖 [DesignGurus — URL Shortener](https://www.designgurus.io/blog/url-shortening)
📖 [HelloInterview — URL Shortener](https://www.hellointerview.com/learn/system-design/problem-breakdowns/tinyurl)
📖 [SystemDesignSchool — URL Shortener](https://systemdesignschool.io/problems/url-shortener/solution)

---

#### HLD-2. Design a Rate Limiter

**Requirements:** Limit requests per client per time window. Distributed (works across multiple servers). Return 429 on exceed.

**Key decisions:**
- **Algorithm:** Token bucket (allows bursts) or sliding window counter (smooth)
- **Storage:** Redis (INCR + EXPIRE for counters)
- **Placement:** API Gateway or middleware
- **Rules:** Configurable per endpoint, per user, per IP

📺 [ByteByteGo — Rate Limiter](https://www.youtube.com/watch?v=FU4WlwfS3G0)
📖 [HelloInterview — Rate Limiter](https://www.hellointerview.com/learn/system-design/problem-breakdowns/rate-limiter)

---

#### HLD-3. Design a Chat System (WhatsApp)

**Requirements:** 1:1 messaging, group chat (up to 100), online/offline status, message delivery/read receipts, offline message storage.

**Key decisions:**
- **Real-time:** WebSocket connections for online users
- **Message flow:** Sender → Chat Server → check if recipient online → deliver via WebSocket OR store for later
- **Storage:** Cassandra/DynamoDB for messages (partition by chat_id, sort by timestamp)
- **Offline delivery:** Store undelivered messages, push notification via APNS/FCM
- **Group chat:** Fan-out message to all group members
- **Ordering:** Sequence IDs per conversation

📺 [Gaurav Sen — Design WhatsApp](https://www.youtube.com/watch?v=vvhC64hQZMk)
📖 [HelloInterview — WhatsApp](https://www.hellointerview.com/learn/system-design/problem-breakdowns/whatsapp)
📖 [ByteByteGo — Chat System](https://bytebytego.com/courses/system-design-interview/design-a-chat-system)

---

#### HLD-4. Design a News Feed (Twitter/Facebook)

**Requirements:** Users follow others, post content, see personalized feed of followed users' posts.

**Key decisions:**
- **Fan-out on write (push):** When user posts, push to all followers' feed cache. Fast reads, expensive writes for celebrities.
- **Fan-out on read (pull):** Compute feed on request by merging recent posts from followed users. Slow reads, cheap writes.
- **Hybrid:** Push for normal users, pull for celebrities (>100K followers).
- **Feed storage:** Pre-computed feed in Redis (sorted set by timestamp).
- **Ranking:** Chronological (simple) or ML-ranked (complex).

📺 [Gaurav Sen — Design Twitter](https://www.youtube.com/watch?v=wYk0xPP_P_8)
📺 [ByteByteGo — News Feed](https://www.youtube.com/watch?v=R_agd5qjc1I)
📖 [DesignGurus — Twitter Design](https://www.designgurus.io/answers/detail/how-to-design-a-twitter-like-application)

---

#### HLD-5. Design a Video Streaming Platform (YouTube/Netflix)

**Requirements:** Upload videos, transcode to multiple resolutions, stream globally with low latency, search, recommendations.

**Key decisions:**
- **Upload:** Chunked upload → message queue → transcoding workers (FFmpeg) → multiple resolutions (360p, 720p, 1080p, 4K)
- **Storage:** Blob storage (S3) for video files, relational DB for metadata
- **Delivery:** CDN for global distribution, adaptive bitrate streaming (HLS/DASH)
- **Streaming protocol:** HLS (HTTP Live Streaming) — video split into small segments, client requests segments sequentially

📺 [ByteByteGo — YouTube Design](https://www.youtube.com/watch?v=jPKTo1iGQiE)
📖 [DesignGurus — Video Streaming](https://www.designgurus.io/blog/design-video-streaming-platform)
📖 [ByteByteGo — YouTube Written](https://bytebytego.com/courses/system-design-interview/design-youtube)

---

#### HLD-6. Design a File Storage Service (Dropbox/Google Drive)

**Requirements:** Upload/download files, sync across devices, notifications on changes, versioning (optional).

**Key decisions:**
- **Chunking:** Split large files into 4MB chunks. Only upload changed chunks (deduplication).
- **Storage:** S3 for file chunks, relational DB for metadata (file tree, versions, permissions)
- **Sync:** Client watches local filesystem → detects changes → uploads changed chunks → notifies other clients via WebSocket/long polling
- **Conflict resolution:** If two clients edit same file, save both versions and let user resolve

📺 [Gaurav Sen — Design Dropbox](https://www.youtube.com/watch?v=U0xTu6E2CT8)
📖 [DesignGurus — Dropbox](https://www.designgurus.io/course-play/grokking-the-system-design-interview/doc/designing-dropbox)

---

#### HLD-7. Design a Ride-Hailing Service (Uber)

**Requirements:** Rider requests ride, match with nearest available driver, real-time tracking, fare calculation, payments.

**Key decisions:**
- **Location tracking:** Drivers send GPS pings every 3-5 seconds → stored in-memory (geospatial index)
- **Geospatial indexing:** Geohash or Quadtree to find nearby drivers efficiently
- **Matching:** Find available drivers within radius → rank by distance/ETA → send request → timeout → try next
- **Real-time updates:** WebSocket for driver location streaming to rider
- **Partitioning:** By city/region for data locality

📺 [Gaurav Sen — Design Uber](https://www.youtube.com/watch?v=umWABit-wbk)
📖 [HelloInterview — Uber](https://www.hellointerview.com/learn/system-design/problem-breakdowns/uber)
📖 [SystemDesignSchool — Uber](https://systemdesignschool.io/problems/uber/solution)

---

#### HLD-8. Design a Notification System

**Requirements:** Send push notifications, SMS, and email. Support millions of users. Prioritization, rate limiting, user preferences.

**Key decisions:**
- **Architecture:** Notification service → message queue (per channel) → channel-specific workers (push/SMS/email)
- **Push:** APNS (iOS), FCM (Android)
- **Deduplication:** Idempotency key to prevent duplicate notifications
- **User preferences:** DB table for opt-in/opt-out per channel per notification type
- **Priority queue:** Urgent notifications (OTP) get higher priority than marketing

📺 [ByteByteGo — Notification System](https://www.youtube.com/watch?v=bBTPZ9NdSk8)

---

#### HLD-9. Design a Web Crawler

**Requirements:** Crawl billions of web pages, extract content, handle politeness (robots.txt), avoid duplicates, handle traps.

**Key decisions:**
- **Architecture:** URL frontier (priority queue) → fetcher workers → content parser → URL extractor → dedup (Bloom filter) → back to frontier
- **Politeness:** Respect robots.txt, rate limit per domain
- **Dedup:** Bloom filter for URL dedup, content hash (SimHash) for near-duplicate detection
- **Storage:** Blob storage for raw HTML, search index for parsed content
- **Scale:** Distributed workers, partition URL frontier by domain

📺 [ByteByteGo — Web Crawler](https://www.youtube.com/watch?v=BKZxZwUgL3Y)

---

#### HLD-10. Design Search Autocomplete / Typeahead

**Requirements:** As user types, suggest top 5-10 completions. Low latency (<100ms). Based on popularity/frequency.

**Key decisions:**
- **Data structure:** Trie (prefix tree) with frequency counts at each node
- **Storage:** In-memory trie on each server, rebuilt periodically from analytics data
- **Ranking:** Top-K by frequency at each prefix node (pre-computed)
- **Updates:** Offline pipeline aggregates search logs → rebuilds trie → deploys to servers
- **Optimization:** Only query after 2-3 characters typed, debounce 200ms

📺 [ByteByteGo — Autocomplete](https://www.youtube.com/watch?v=us0qySiUsGU)

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

**Requirements:** Ingest logs/events from thousands of services, process in real-time and batch, store for querying, support dashboards and alerting.

**Key components:**
```
Services → Log Agents (Fluentd/Filebeat) → Kafka (ingestion buffer)
                                                ↓
                              ┌─────────────────┼─────────────────┐
                              ↓                 ↓                 ↓
                    Stream Processing     Batch Processing    Raw Storage
                    (Flink/Spark Streaming) (Spark/EMR)       (S3/HDFS)
                              ↓                 ↓
                    Real-time Alerts     Data Warehouse
                    (PagerDuty)          (Redshift/BigQuery)
                              ↓                 ↓
                         Dashboards        Ad-hoc Queries
                         (Grafana)         (Athena/Presto)
```

**Key decisions:**
- **Lambda vs Kappa architecture:** Lambda = separate batch + stream paths (more complex, handles late data well). Kappa = stream-only with replay capability (simpler, Kafka-native). For most cases, Kappa with Kafka's retention is sufficient.
- **Exactly-once processing:** Kafka + Flink with checkpointing provides exactly-once semantics. Critical for metrics accuracy.
- **Schema evolution:** Use Avro/Protobuf with a schema registry. Backward-compatible changes don't break consumers.
- **Partitioning:** Partition Kafka topics by service_name or timestamp for parallelism.
- **Retention:** Hot data (7 days) in Kafka, warm data (90 days) in S3 Standard, cold data (1+ year) in S3 Glacier.

**Interview talking point:** "I'd use a Kappa architecture with Kafka as the central ingestion layer. Log agents on each service push structured events to Kafka topics partitioned by service name. Flink consumers process the stream for real-time alerting (error rate spikes), while a separate consumer writes to S3 in Parquet format for batch analytics via Athena. This gives us both real-time and historical querying without maintaining two separate pipelines."

📺 [ByteByteGo — Log Aggregation](https://www.youtube.com/watch?v=rnZmdmlR-2M)

---

#### HLD-32. Design a Feature Flags / A/B Testing Platform

**Requirements:** Toggle features on/off without deployment, target specific user segments, run A/B experiments with statistical significance, track metrics per variant.

**Key components:**
```
SDK (client-side) → Feature Flag Service → Flag Store (Redis + DB)
                          ↓
                    Targeting Rules Engine
                    (user_id, %, geo, attributes)
                          ↓
                    Event Collector → Analytics Pipeline
                          ↓
                    Experiment Dashboard (p-value, confidence interval)
```

**Key decisions:**
- **Evaluation:** Client-side (SDK caches flags, evaluates locally — fast, works offline) vs server-side (API call per evaluation — always fresh, more control). Hybrid: SDK fetches flag config periodically, evaluates locally.
- **Consistent assignment:** Hash `user_id + experiment_id` to deterministically assign users to variants. Same user always sees same variant.
- **Gradual rollout:** Start at 1% → 5% → 25% → 100%. If error rate spikes, auto-rollback.
- **Statistical significance:** Track conversion metrics per variant. Use Bayesian or frequentist analysis. Minimum sample size before declaring a winner.
- **Flag lifecycle:** Active → Completed → Archived. Stale flags (>90 days) should be cleaned up to reduce tech debt.

**Interview talking point:** "The SDK fetches flag configurations every 60 seconds and caches locally. For each flag evaluation, it hashes `user_id + flag_key` to get a consistent percentage bucket — so user assignment is deterministic without server calls. Events (flag evaluated, conversion happened) are batched and sent to Kafka for the analytics pipeline. The experiment dashboard shows conversion rates per variant with confidence intervals."

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
- **Model serving:** TensorRT, vLLM (for LLMs), Triton Inference Server. Key: KV cache management, model parallelism (tensor/pipeline) for large models.
- **Auto-scaling:** Scale GPU instances based on queue depth, not CPU. GPU utilization target: 70-80%.

#### HLD-35. Design a Real-Time Analytics Pipeline
Kafka → Flink/Spark Streaming → aggregation → time-series DB → dashboard. Lambda vs Kappa architecture.

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

```python
from abc import ABC, abstractmethod
from enum import Enum
from datetime import datetime

class VehicleType(Enum):
    MOTORCYCLE = 1
    CAR = 2
    TRUCK = 3

class SpotSize(Enum):
    SMALL = 1      # motorcycle
    MEDIUM = 2     # car
    LARGE = 3      # truck

class PricingStrategy(ABC):
    """Strategy pattern — swap pricing without changing ParkingLot"""
    @abstractmethod
    def calculate_fee(self, hours: float, spot_size: SpotSize) -> float:
        pass

class HourlyPricing(PricingStrategy):
    RATES = {SpotSize.SMALL: 2.0, SpotSize.MEDIUM: 5.0, SpotSize.LARGE: 10.0}
    def calculate_fee(self, hours: float, spot_size: SpotSize) -> float:
        return self.RATES[spot_size] * max(1, hours)  # minimum 1 hour

class ParkingSpot:
    def __init__(self, spot_id: str, size: SpotSize, floor: int):
        self.spot_id = spot_id
        self.size = size
        self.floor = floor
        self.vehicle = None

    def is_available(self) -> bool:
        return self.vehicle is None

    def park(self, vehicle) -> bool:
        if self.is_available() and vehicle.fits(self.size):
            self.vehicle = vehicle
            return True
        return False

    def remove_vehicle(self):
        self.vehicle = None

class Vehicle:
    def __init__(self, license_plate: str, v_type: VehicleType):
        self.license_plate = license_plate
        self.v_type = v_type

    def fits(self, spot_size: SpotSize) -> bool:
        """A vehicle fits in a spot of equal or larger size"""
        return spot_size.value >= self.v_type.value

class Ticket:
    def __init__(self, vehicle: Vehicle, spot: ParkingSpot):
        self.vehicle = vehicle
        self.spot = spot
        self.entry_time = datetime.now()

class ParkingLot:
    """Singleton — one instance per physical lot"""
    _instance = None

    def __init__(self, floors: int, spots_per_floor: dict):
        self.floors = {}  # floor_num -> list of ParkingSpot
        self.active_tickets = {}  # license_plate -> Ticket
        self.pricing = HourlyPricing()
        # ... initialize floors and spots

    def find_spot(self, vehicle: Vehicle) -> ParkingSpot | None:
        """Find nearest available spot that fits the vehicle"""
        for floor_num in sorted(self.floors.keys()):
            for spot in self.floors[floor_num]:
                if spot.is_available() and vehicle.fits(spot.size):
                    return spot
        return None  # lot full

    def issue_ticket(self, vehicle: Vehicle) -> Ticket | None:
        spot = self.find_spot(vehicle)
        if not spot:
            return None
        spot.park(vehicle)
        ticket = Ticket(vehicle, spot)
        self.active_tickets[vehicle.license_plate] = ticket
        return ticket

    def process_exit(self, license_plate: str) -> float:
        ticket = self.active_tickets.pop(license_plate)
        hours = (datetime.now() - ticket.entry_time).total_seconds() / 3600
        fee = self.pricing.calculate_fee(hours, ticket.spot.size)
        ticket.spot.remove_vehicle()
        return fee
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

```python
class Node:
    def __init__(self, key: int, val: int):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}  # key -> Node
        # Dummy head/tail to avoid null checks
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def _add_to_front(self, node: Node):
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self._remove(node)
        self._add_to_front(node)  # mark as recently used
        return node.val

    def put(self, key: int, value: int):
        if key in self.cache:
            self._remove(self.cache[key])
        node = Node(key, value)
        self._add_to_front(node)
        self.cache[key] = node
        if len(self.cache) > self.capacity:
            lru = self.tail.prev  # least recently used
            self._remove(lru)
            del self.cache[lru.key]
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

```python
from enum import Enum
from abc import ABC, abstractmethod
import heapq

class Direction(Enum):
    UP = 1
    DOWN = -1
    IDLE = 0

class RequestType(Enum):
    EXTERNAL = 1  # from floor button
    INTERNAL = 2  # from inside elevator

class Request:
    def __init__(self, floor: int, direction: Direction, req_type: RequestType):
        self.floor = floor
        self.direction = direction
        self.req_type = req_type

# --- State Pattern: Elevator behavior changes based on state ---
class ElevatorState(ABC):
    @abstractmethod
    def handle_request(self, elevator: 'Elevator', request: Request): pass

class IdleState(ElevatorState):
    def handle_request(self, elevator: 'Elevator', request: Request):
        if request.floor == elevator.current_floor:
            elevator.open_door()
        elif request.floor > elevator.current_floor:
            elevator.direction = Direction.UP
            elevator.state = MovingState()
            elevator.add_stop(request.floor)
        else:
            elevator.direction = Direction.DOWN
            elevator.state = MovingState()
            elevator.add_stop(request.floor)

class MovingState(ElevatorState):
    def handle_request(self, elevator: 'Elevator', request: Request):
        # Accept request if it's on the way (SCAN algorithm behavior)
        elevator.add_stop(request.floor)

class DoorOpenState(ElevatorState):
    def handle_request(self, elevator: 'Elevator', request: Request):
        elevator.add_stop(request.floor)  # queue it

# --- Strategy Pattern: Scheduling algorithm is swappable ---
class SchedulingStrategy(ABC):
    @abstractmethod
    def select_elevator(self, elevators: list['Elevator'], request: Request) -> 'Elevator':
        pass

class LookStrategy(SchedulingStrategy):
    """LOOK algorithm: pick elevator closest to request floor, moving in same direction"""
    def select_elevator(self, elevators: list['Elevator'], request: Request) -> 'Elevator':
        best, best_score = None, float('inf')
        for e in elevators:
            if e.is_maintenance:
                continue
            distance = abs(e.current_floor - request.floor)
            # Prefer: idle elevators, then same-direction, then closest
            if e.direction == Direction.IDLE:
                score = distance
            elif e.direction == request.direction:
                if (e.direction == Direction.UP and e.current_floor <= request.floor) or \
                   (e.direction == Direction.DOWN and e.current_floor >= request.floor):
                    score = distance  # on the way
                else:
                    score = distance + 1000  # wrong side
            else:
                score = distance + 2000  # opposite direction
            if score < best_score:
                best_score = score
                best = e
        return best

class Elevator:
    def __init__(self, elevator_id: int):
        self.id = elevator_id
        self.current_floor = 0
        self.direction = Direction.IDLE
        self.state: ElevatorState = IdleState()
        self.stops: list[int] = []  # sorted stops
        self.is_maintenance = False

    def add_stop(self, floor: int):
        if floor not in self.stops:
            self.stops.append(floor)
            self.stops.sort(reverse=(self.direction == Direction.DOWN))

    def open_door(self):
        self.state = DoorOpenState()
        # Remove current floor from stops, notify observers (displays)
        if self.current_floor in self.stops:
            self.stops.remove(self.current_floor)

    def move(self):
        """Called by system tick — moves elevator one floor"""
        if not self.stops:
            self.direction = Direction.IDLE
            self.state = IdleState()
            return
        self.state = MovingState()
        self.current_floor += self.direction.value
        if self.current_floor == self.stops[0]:
            self.open_door()

class ElevatorSystem:
    def __init__(self, num_elevators: int, num_floors: int, strategy: SchedulingStrategy = None):
        self.elevators = [Elevator(i) for i in range(num_elevators)]
        self.num_floors = num_floors
        self.strategy = strategy or LookStrategy()

    def request_elevator(self, request: Request):
        elevator = self.strategy.select_elevator(self.elevators, request)
        elevator.state.handle_request(elevator, request)
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
Classes: `Hotel`, `Room`, `RoomType`, `Reservation`, `Guest`, `Payment`. Key: date-range availability check, overbooking prevention.

#### LLD-10. Design a Movie Ticket Booking System
Classes: `Cinema`, `Hall`, `Show`, `Seat`, `Booking`, `Payment`. Key: seat locking during selection (5-min timeout).

📖 [DesignGurus — Movie Ticket](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-a-movie-ticket-booking-system)

#### LLD-11. Design an In-Memory File System
Classes: `FileSystem`, `Directory`, `File`, `Path`. Key: tree structure, path resolution, CRUD operations.

#### LLD-12. Design a Logging Framework
Classes: `Logger`, `LogLevel`, `LogHandler` (Console/File/Remote), `LogFormatter`. Patterns: Singleton (Logger), Chain of Responsibility (handlers), Strategy (formatters).

#### LLD-13. Design a Pub/Sub System (In-Process)
Classes: `EventBus`, `Topic`, `Publisher`, `Subscriber`, `Message`. Pattern: Observer.

#### LLD-14. Design a Task Scheduler
Classes: `Scheduler`, `Task`, `Trigger` (cron/one-time), `Worker`, `TaskQueue`. Key: priority queue, thread pool.

#### LLD-15. Design a Shopping Cart
Classes: `Cart`, `CartItem`, `Product`, `User`, `Coupon`, `PriceCalculator`. Pattern: Strategy (discount calculation).

#### LLD-16. Design an ATM Machine
Classes: `ATM`, `Account`, `Card`, `Transaction`, `CashDispenser`, `Screen`. Pattern: State (idle → card_inserted → pin_entered → transaction_selected).

📖 [DesignGurus — ATM](https://www.designgurus.io/course-play/grokking-the-object-oriented-design-interview/doc/design-an-atm)

#### LLD-17. Design a Food Ordering System
Classes: `Restaurant`, `Menu`, `MenuItem`, `Order`, `OrderItem`, `Customer`, `DeliveryAgent`, `Payment`.

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
#### LLD-22. Design a Notification Service (Class Level)
#### LLD-23. Design a Rate Limiter (Implementation)
#### LLD-24. Design a Thread-Safe Circular Buffer
#### LLD-25. Design a Cache with TTL Support

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
| **Day 2** (Tue) | 2.5 hrs | Skim concepts you DON'T know from work | Scan Part 1 concepts #1-54. Skip what you already use at Amazon. Deep-read only: CAP theorem, consistent hashing, fan-out strategies, cache stampede, quorum. |
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
| **Day 14** (Sun) | 4 hrs | Mock interview #3 + cheat sheet review | Final mock. Then review: decision matrices (DB, communication, scaling) from Part 8. Review follow-up questions from concept #54. Read company-specific notes for your target. |

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

## Quick Reference: Cheat Sheet

### Problem → Key Concepts Map (Study Guide)

> Use this to know which concepts to review before practicing each problem.

| Problem | Key Concepts You Need |
|---------|----------------------|
| **HLD-1. URL Shortener** | #6 (estimation), #7 (SQL vs NoSQL), #14 (consistent hashing), #19 (caching), #35 (idempotency) |
| **HLD-2. Rate Limiter** | #4 (rate limiting algorithms), #5 (API gateway), #21 (Redis) |
| **HLD-3. WhatsApp** | #26 (WebSockets), #27 (WS at scale), #28 (message queues), #36 (eventual consistency), #18 (data modeling) |
| **HLD-4. News Feed** | #19 (caching), #29 (pub/sub), #21 (Redis sorted sets), #8 (sharding) |
| **HLD-5. YouTube** | #17 (blob storage), #22 (CDN), #28 (message queues), #6 (estimation) |
| **HLD-6. Dropbox** | #17 (blob storage), #26 (WebSockets), #35 (idempotency), #9 (replication) |
| **HLD-7. Uber** | #10 (geospatial indexing), #26 (WebSockets), #28 (message queues), #8 (sharding) |
| **HLD-8. Notifications** | #28 (message queues), #29 (pub/sub), #35 (idempotency), #4 (rate limiting) |
| **HLD-9. Web Crawler** | #42 (bloom filters), #28 (message queues), #17 (blob storage) |
| **HLD-10. Autocomplete** | #19 (caching), #6 (estimation), #10 (indexing) |
| **LLD-1. Parking Lot** | Strategy pattern, Factory, Singleton, SOLID |
| **LLD-2. Elevator** | State pattern, Strategy pattern, Observer |
| **LLD-5. LRU Cache** | HashMap + Doubly Linked List, #60 (thread safety for follow-up) |
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

> **Total items:** 69 concepts + 37 HLD problems + 25 LLD problems + 14 design patterns + SOLID principles + trade-off framework + worked examples = **155+ unique topics**
>
> **Every item sourced from real FAANG interview data (Glassdoor, Blind, onsites.fyi, interviewing.io, candidate experiences).**
>
> **This guide covers ~95% of what's asked at SDE2 level. For the remaining ~5% (novel problems), the 4-step framework, trade-off discussion skills, and foundational concepts transfer directly. The framework matters more than the problem list.**
