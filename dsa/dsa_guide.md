# DSA — The Practical Developer's Guide

> **Who is this for?** You can build real projects. You ship code at work. But DSA feels like
> a different universe — boring, abstract, disconnected from reality. You want to crack
> interviews without grinding 500 problems like a robot.
>
> **The truth:** DSA is just problem-solving with names. You already do it daily — you just
> don't call it "sliding window" when you scan through logs looking for an error pattern.
>
> **This guide:** Follow it line by line. Each step builds on the previous one. Every pattern
> is connected to something real. Links included for every concept so you never get stuck.

---

## The Only Thing That Matters

> You need to solve 2 medium problems in 45 minutes while talking out loud.
> That's the entire interview. Everything in this guide exists to get you there.

### Do I Need to Know Every Way to Solve a Problem?

**No.** You need ONE reliable approach per pattern that you can execute fast and explain clearly.

| Situation | What You Need |
|---|---|
| Easy problem | One clean solution. Don't overthink it. |
| Medium problem | Brute force → optimal. Know how to go from O(n²) to O(n). |
| Hard problem | One working approach. If interviewer wants another, they'll hint. |
| "Can you solve it differently?" | Say: "Yes, we could also use [X], which trades [time/space]." You don't need to code it — just mention it. |

**Knowing one approach deeply > knowing three approaches shallowly.**

A clean, well-communicated O(n) solution with good edge case handling will ALWAYS beat a messy O(n) solution where you also mentioned two other approaches but ran out of time.

### Is This Guide Enough for FAANG + Top Companies?

**Yes.** Google, Meta, Amazon, Apple, Microsoft, Atlassian, Airbnb, Stripe, Uber, Netflix — they all pull from the same pool of ~200 problems across the same ~14 patterns. This guide covers all of them.

**What gets you hired:**
- ✅ Identify the pattern in 60 seconds
- ✅ Code the optimal solution in 15-20 minutes
- ✅ Talk through your logic while coding
- ✅ Handle edge cases without being asked
- ✅ State time/space complexity before and after coding

**What does NOT get you hired:**
- ❌ Knowing 5 ways to solve Two Sum
- ❌ Solving 800 LeetCode problems
- ❌ Memorizing solutions
- ❌ Knowing segment trees or advanced math

### When to Stop Preparing

You're ready when:
- You can solve 80% of medium LeetCode problems in under 20 minutes
- You can identify the pattern for any new problem within 60 seconds
- You can explain your approach before coding it
- You've done at least 3 timed mock interviews

**Stop grinding and go interview.** Over-preparation is real. After ~120-150 problems with pattern mastery, more problems have diminishing returns. Your time is better spent doing mock interviews.

### What This Guide Does NOT Cover

This guide is for **DSA coding rounds only.** Top companies also have:
- **System Design rounds** (for mid/senior roles) — separate preparation needed
- **Behavioral rounds** (Amazon LPs, Meta "Jedi" round) — prepare STAR stories separately
- **Practical coding rounds** (Stripe, Airbnb sometimes ask "build a feature") — different from DSA

DSA is usually 2 out of 4-5 interview rounds. This guide makes those 2 rounds a guaranteed pass.

### What to Say When You're Completely Stuck

This WILL happen. Even to the best. Here's the script:

```
DON'T: Go silent. Stare at the screen. Panic.

DO: Say one of these:
→ "Let me think about this from a different angle. What if I try [X] instead?"
→ "I'm not seeing the optimal approach yet. Let me start with brute force
   and optimize from there."
→ "Can I ask — is the input always positive / sorted / connected?"
   (Clarifying questions buy time AND often unlock the insight)
→ "I think this might be a [pattern] problem because [signal], but I'm
   not 100% sure. Let me try it and see."
```

Interviewers WANT to help you. Thinking out loud — even when stuck — gives them a chance to nudge you. Silent candidates get no help.

### If You Only Have 2 Weeks

Skip to the essentials. Do ONLY these 6 patterns (they cover 80% of all interview questions):
1. HashMap (Phase 1.1) — 5 problems
2. Two Pointers (Phase 1.2) — 5 problems
3. Sliding Window (Phase 1.3) — 4 problems
4. Binary Search (Phase 2.1) — 4 problems
5. BFS/DFS on Trees + Graphs (Phase 4 + 6) — 10 problems
6. Dynamic Programming 1D + String (Phase 7.1 + 7.3) — 8 problems

**Total: ~36 problems in 2 weeks.** Not ideal, but enough to pass most interviews.

---

## What Interviewers Actually Score You On

> This is the cheat code. Most candidates think interviews are about "getting the right answer."
> They're not. Interviewers score you on **5 signals:**

| Signal | What It Means | Weight |
|---|---|---|
| 1. Pattern Recognition Speed | How fast you identify the approach | 20% |
| 2. Complexity Awareness | You state time/space BEFORE coding | 20% |
| 3. Edge Case Discipline | You catch empty, null, overflow, duplicates | 20% |
| 4. Communication Clarity | You explain your thinking out loud as you go | 20% |
| 5. Recovery After Wrong Start | You pivot gracefully, not panic | 20% |

**The interviewer's internal checklist:**
- ✅ Did they ask clarifying questions before coding?
- ✅ Did they propose brute force first, THEN optimize?
- ✅ Did they state complexity before writing code?
- ✅ Did they talk through their logic while coding?
- ✅ Did they test with examples and edge cases after coding?
- ✅ When stuck, did they reason through it or just freeze?

**The response format you should use in EVERY interview:**
```
1. Restate the problem in one line
2. Ask: "Can the input be empty? Negative? Duplicates? Sorted?"
3. Propose brute force: "The naive approach would be O(n²) by..."
4. Optimize: "But if we use [pattern], we can do O(n) because..."
5. State: "Time: O(n), Space: O(n)"
6. Code it — talk while you write
7. Dry run with the given example
8. Test edge cases: empty, single element, all same
```

> 🔗 Deep dive: [What the Perfect FAANG Coding Interview Looks Like](https://grokkingtechcareer.substack.com/p/what-does-the-perfect-faang-coding)

---

## Before You Touch Any Problem

### Step 1: Change How You Think About DSA

DSA is not academic theory. It's the same thinking you use when:
- You **search** your email inbox → that's **binary search** (sorted by date, you jump to the middle)
- You **undo** actions in your editor → that's a **stack** (last in, first out)
- You find the **shortest route** on Google Maps → that's **BFS/Dijkstra**
- You **cache** API responses to avoid re-fetching → that's a **hashmap**
- You **scroll** through a feed loading 10 items at a time → that's a **sliding window**
- You **retry** a failed request with backoff → that's **recursion** with a base case

Every pattern below maps to something you already understand. We'll connect the dots.

### Step 2: Learn How to Dry Run (The #1 Skill Nobody Teaches)

Before you write a single line of code, you need to be able to **trace through logic on paper**.

**How to dry run:**
1. Write down the input
2. Create a table with columns for each variable
3. Step through each line of code, updating the table
4. Track what changes at each step

**Example — Two Sum with hashmap:**
```
Input: [2, 7, 11, 15], target = 9

Step | i | nums[i] | complement | map          | result
-----|---|---------|------------|--------------|-------
1    | 0 | 2       | 7          | {2: 0}       | -
2    | 1 | 7       | 2          | 2 is in map! | [0, 1] ✓
```

> 📺 Watch: [How to Dry Run Code](https://www.youtube.com/results?search_query=how+to+dry+run+code+dsa) — pick any 10-min video, the skill is universal.

**Practice this on EVERY problem for the first 2 weeks.** After that, you'll do it in your head automatically.

### Step 3: Understand Time & Space Complexity (30 Minutes, Not More)

You don't need to prove theorems. You need to **estimate** whether your solution will pass.

**The only rule:** Computers do ~10⁸ operations per second. If your solution does more than that, it's too slow.

| If n = | Your solution must be | What that means |
|---|---|---|
| 20 | O(2ⁿ) is fine | Try all combinations (backtracking) |
| 500 | O(n³) is fine | Triple nested loop |
| 10,000 | O(n²) is fine | Double nested loop |
| 1,000,000 | O(n log n) needed | Sort + binary search, or heap |
| 100,000,000 | O(n) needed | Single pass — two pointers, sliding window |

> 📺 Watch: [Big O Notation — Abdul Bari](https://www.youtube.com/watch?v=9TlHvipP5yA&list=PLKEhaiVaGhXIBTDi4EKKQT7lomRtFqFSB)
> 🔗 Read: [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)

**That's it.** You now know enough complexity analysis. Move on.

---

## The Pattern Recognition Cheat Sheet

> Print this. Tape it to your wall. Use it for every problem until it's muscle memory.

```
Input is SORTED?
  → Two Pointers or Binary Search

Need CONTIGUOUS subarray/substring?
  → Sliding Window or Prefix Sum

Need TOP K or Kth LARGEST?
  → Heap

CONNECTED things, SHORTEST PATH, LEVELS?
  → Graph: BFS (shortest) or DFS (explore all)

ALL COMBINATIONS or PERMUTATIONS?
  → Backtracking

"MINIMUM of MAXIMUM" or "MAXIMUM of MINIMUM"?
  → Binary Search on Answer

OPTIMAL VALUE with CHOICES at each step?
  → DP (if overlapping subproblems) or Greedy (if local = global)

MATCHING PAIRS, NESTING, NEAREST PREVIOUS/NEXT?
  → Stack

Need O(1) LOOKUP or COUNTING?
  → HashMap
```

> 🔗 Interactive version: [LeetCode Pattern Decision Tree](https://leetcopilot.dev/blog/how-to-know-which-algorithm-to-use-on-leetcode)
> 🔗 Deep dive: [AlgoMaster — 20 Patterns](https://blog.algomaster.io/p/20-dsa-patterns)

---


---

## The Grandmaster's Invisible Thinking Process

> This section is the result of studying how top competitive programmers (Codeforces red/grandmaster
> rated) and FAANG senior engineers actually think when they see a new problem. These are the
> cognitive steps they do SO FAST it looks like intuition — but it's a learnable process.
>
> Sources: Codeforces blogs from red coders, cognitive science research on expert programmers,
> FAANG interviewer coaching notes, and competitive programming training methodology.

### The Core Insight Most People Miss

**Novices see:** "Given an array of integers, find two numbers that add up to target."
**Experts see:** "Lookup problem. HashMap. O(n). Done."

The difference isn't intelligence. It's that experts have **compressed** the problem into a pattern they've seen before. They skip steps 1-5 because they've done them thousands of times and the pattern recognition is now instant.

**Your goal:** Make this compression happen for YOU by doing steps 1-9 explicitly until they become automatic.

### The 9-Step Process (Do This For Every Problem)

```
Step 1: STRIP THE STORY
Step 2: PLAY WITH TINY EXAMPLES
Step 3: SPOT THE PATTERN / OBSERVE
Step 4: REDUCE TO A KNOWN PROBLEM
Step 5: BRUTE FORCE FIRST
Step 6: FIND THE BOTTLENECK
Step 7: FIX THE BOTTLENECK WITH A PATTERN
Step 8: VERIFY WITH EDGE CASES
Step 9: CODE IT CLEAN
```

Let's break each one down with what's actually happening in the expert's brain:

---

### Step 1: STRIP THE STORY (30 seconds)

**What experts do:** They ignore the story and extract the mathematical core.

The problem says: "A farmer has N cows standing in a line. Each cow has a happiness value..."
Expert's brain: "Array of N integers."

The problem says: "Find the minimum number of swaps to sort the array..."
Expert's brain: "Cycle detection in permutation. Graph problem."

**Your practice:** After reading any problem, write ONE sentence:
"I have a [data structure] and I need to find [what]."

Example: "I have an array of integers and I need to find two elements that sum to target."

---

### Step 2: PLAY WITH TINY EXAMPLES (1-2 minutes)

> **This is the step geniuses forget to mention.** They do it in their head in 3 seconds.
> You need to do it on paper until it becomes automatic.

**Try the smallest possible inputs:**
- n = 0 (empty) — what should the answer be?
- n = 1 (single element) — trivial case?
- n = 2 — simplest non-trivial case
- n = 3, 4, 5 — do you see a pattern forming?

**Example — Longest Increasing Subsequence:**
```
[10, 9, 2, 5, 3, 7, 101, 18]

Let me try by hand:
- Starting from 2: 2, 5, 7, 101 → length 4
- Starting from 2: 2, 3, 7, 101 → length 4
- Starting from 2: 2, 5, 7, 18 → length 4
- Answer: 4

What am I doing? At each number, I'm asking "what's the longest
sequence ending here?" That depends on all previous numbers...
→ This smells like DP. State: dp[i] = LIS ending at index i.
```

**The magic:** By working through tiny examples BY HAND, you discover the algorithm yourself. You don't need to memorize it — you derive it.

---

### Step 3: SPOT THE PATTERN / OBSERVE (1-2 minutes)

**What experts do:** They look for INVARIANTS — things that stay true no matter what.

**Observation techniques used by red coders:**
1. **What stays constant?** (sum, count, parity, relative order)
2. **What changes?** (which elements move, what grows/shrinks)
3. **Is there a monotonic property?** (if X works, does X+1 also work?)
4. **Can I sort it?** (does order matter, or can I rearrange?)
5. **What's the constraint telling me?** (n ≤ 10⁵ means O(n log n) is needed)

**Example — Container With Most Water:**
```
Observation: If I move the TALLER line inward, I can only lose area
(width decreases, height can't increase past the shorter line).
So I should always move the SHORTER line inward.
→ Two pointers, move the shorter one. Greedy.
```

---

### Step 4: REDUCE TO A KNOWN PROBLEM (30 seconds)

**What experts do:** "This is just [known problem] in disguise."

**Common reductions:**
- "Find if path exists between A and B" → Graph BFS/DFS
- "Find shortest path" → BFS (unweighted) or Dijkstra (weighted)
- "Find all valid configurations" → Backtracking
- "Optimize with choices at each step" → DP
- "Find something in sorted space" → Binary Search
- "Process in LIFO order" → Stack
- "Minimum/maximum of sliding range" → Monotonic deque
- "Group connected things" → Union-Find
- "Order with dependencies" → Topological Sort

If you can't reduce it → go to Step 5 (brute force).

---

### Step 5: BRUTE FORCE FIRST (Always)

**Why:** Even if it's O(n³), writing brute force does three things:
1. Proves you understand the problem
2. Gives you a correct solution to test against
3. Shows the interviewer you can make progress

**In interviews, ALWAYS state brute force first:**
"The naive approach would be to check all pairs, which is O(n²). But we can do better..."

---

### Step 6: FIND THE BOTTLENECK (The Key Insight)

**What experts do:** They look at the brute force and ask: "What makes this slow?"

| Bottleneck | What's Happening | Fix |
|---|---|---|
| Nested loop searching for a value | Scanning array for each element | HashMap for O(1) lookup |
| Nested loop comparing all pairs | Checking every pair | Sort + Two Pointers |
| Recalculating subarray sum | Recomputing from scratch each time | Prefix Sum |
| Recomputing overlapping subproblems | Same recursive call made multiple times | Memoization (DP) |
| Scanning entire array for min/max | Linear scan inside a loop | Heap or Monotonic Stack |
| Trying all subsets | Exponential exploration | Prune with backtracking or use DP |

> 🔗 Read: [How to Know Which Algorithm to Use](https://leetcopilot.dev/blog/how-to-know-which-algorithm-to-use-on-leetcode)

---

### Step 7: FIX THE BOTTLENECK WITH A PATTERN

This is where your pattern knowledge kicks in. The bottleneck tells you which pattern to use.

**The fix is almost always one of these:**
- Replace inner loop with HashMap → O(n)
- Sort + Two Pointers → O(n log n)
- Sliding Window → O(n)
- Prefix Sum → O(n) preprocessing, O(1) queries
- Binary Search → O(log n) per query
- Heap → O(log n) per insert/extract
- Memoization → eliminate redundant computation

---

### Step 8: VERIFY WITH EDGE CASES (Before Coding!)

**The edge case checklist experts run mentally:**
- Empty input (n = 0)
- Single element (n = 1)
- All elements identical
- Already sorted / reverse sorted
- All negative numbers
- Maximum constraint values (will it overflow?)
- Duplicate elements (does your logic handle them?)

**For specific data structures:**
- Trees: null root, single node, skewed (linked-list-like)
- Graphs: disconnected components, self-loops, single node
- Strings: empty string, single character, all same character
- Linked Lists: empty list, single node, cycle

---

### Step 9: CODE IT CLEAN

**How experts write code differently from beginners:**
- Variable names describe what they hold (`left`, `right`, `count`, `maxLen` — not `i`, `j`, `k`, `x`)
- They write in small blocks, testing each block mentally
- They handle edge cases FIRST (early returns for empty/trivial input)
- They talk while coding: "Now I'm initializing the hashmap... iterating through the array..."

---

## How Experts Visualize Each Data Structure

> Research shows expert programmers use VISUAL MENTAL IMAGERY. They literally "see"
> the data structure in their mind. Here's what they see:

| Data Structure | What Experts Visualize |
|---|---|
| Array | A row of numbered boxes. Two pointers as arrows moving along it. |
| HashMap | A phonebook — look up a name, instantly get the number. |
| Stack | A stack of plates. You can only touch the top one. |
| Queue | A line of people. First in, first out. |
| Linked List | A chain of paper clips. Each one points to the next. |
| Binary Tree | An upside-down family tree. Each person has at most 2 children. |
| Graph | A map of cities connected by roads. |
| Heap | A tournament bracket. The winner (min/max) is always at the top. |
| Trie | An autocomplete dropdown. Each keystroke narrows the options. |

**Practice this:** When you read a problem, CLOSE YOUR EYES and visualize the data structure. See the pointers moving. See the window sliding. See the BFS wave expanding level by level. This is what makes experts fast — they're running a simulation in their head.

> 🔗 Visualize it for real: [VisuAlgo](https://visualgo.net/) | [Algorithm Visualizer](https://algorithm-visualizer.org/)

---

## The "Observation Muscle" — How to Build It

> From Codeforces: "Observation means that you stare at some problem for a sufficient
> amount of time and you are able to reduce it to some easier problem."

**How to train this muscle:**

1. **After reading the editorial, DON'T just learn the solution.** Reconstruct the THINKING PATH:
   "What observation would I need to make to arrive at this solution? What should I have noticed?"

2. **Keep an "Observations Log."** After every problem, write:
   - What was the key observation?
   - What signal in the problem should have triggered it?
   - What pattern did it reduce to?

   Example:
   ```
   Problem: Container With Most Water
   Key observation: Moving the taller line inward can only decrease area
   Signal: "maximize" + two boundaries + greedy choice
   Pattern: Two Pointers (opposite direction)
   ```

3. **The "What If" game:** After solving a problem, ask yourself:
   - What if the array was sorted? (Would two pointers work?)
   - What if I needed the answer for every prefix? (Prefix sum?)
   - What if the constraint was 10x larger? (Need a faster algorithm?)
   - What if there were negative numbers? (Does my solution still work?)

   This is EXACTLY what interviewers do when they ask follow-up questions. If you've already asked yourself these questions, you'll answer instantly.

---

## The Gap Between You and a Grandmaster (It's Smaller Than You Think)

| What You Do Now | What a Grandmaster Does | The Gap |
|---|---|---|
| Read problem, feel lost, try random things | Read problem, strip story, try tiny examples | They have a PROCESS. You can learn it. |
| See "array" and think "loop" | See "array" + "contiguous" and think "sliding window" | They have TRIGGER WORDS mapped to patterns. You can memorize them. |
| Write code, then think about edge cases | Think about edge cases BEFORE coding | They have a CHECKLIST. You can copy it. |
| Get stuck and freeze | Get stuck, say "let me try a different approach" | They have RECOVERY STRATEGIES. You can practice them. |
| Solve the problem and move on | Solve, then ask "what if the constraint changed?" | They do POST-MORTEM. You can start doing it. |
| Memorize solutions | Understand WHY the solution works | They learn PATTERNS, not problems. You can too. |

**The honest truth:** A grandmaster has done steps 1-9 thousands of times until it's automatic. You need to do it explicitly ~150 times (the problems in this guide). After that, you'll have the same pattern recognition for interview-level problems. You won't be a grandmaster at competitive programming — but you'll be indistinguishable from one in a 45-minute interview.

> 🔗 Read: [The Science of Training in Competitive Programming](https://codeforces.com/blog/entry/17842) — the original Codeforces blog that explains the "gap jumping" framework
> 🔗 Read: [AlgoMaster — 20 DSA Patterns](https://blog.algomaster.io/p/20-dsa-patterns) — reusable templates for each pattern
> 🔗 Read: [Colin Galen — Beginner to Grandmaster](https://www.youtube.com/watch?v=bSdp2WeyuJY) — a grandmaster's roadmap


---

## The Problem Setter's Secret (Reverse-Engineer Any Problem)

> Problem setters don't start with a story. They start with an ALGORITHM, then wrap
> it in a story and set constraints to force that specific solution. If you learn to
> read problems backwards — from constraints to algorithm — you skip the puzzle entirely.

### How Every DSA Problem Is Born

```
Setter's process:
1. Pick an algorithm (e.g., sliding window)
2. Design a scenario that requires it (e.g., "longest substring")
3. Set constraints to force it (n = 10^5 → must be O(n), so brute force O(n²) fails)
4. Add a story to disguise it ("a farmer has cows...")
5. Add edge cases to break naive solutions
```

**Your reverse process:**
```
1. Read constraints FIRST (n = 10^5 → need O(n log n) or better)
2. Strip the story → extract the mathematical core
3. Match constraint to algorithm family
4. Confirm with signal words in the problem
```

### The Constraint Decoder (The Setter's Fingerprint)

| Constraint | Setter intended | Algorithm family |
|---|---|---|
| n ≤ 15-20 | Exponential is fine | Bitmask, brute force, backtracking |
| n ≤ 100 | O(n³) is fine | Triple loop, Floyd-Warshall |
| n ≤ 1,000 | O(n²) is fine | DP with 2D table, brute force with pruning |
| n ≤ 10,000 | O(n²) is tight | DP, two pointers |
| n ≤ 100,000 | O(n log n) needed | Sort + binary search, heap, merge sort |
| n ≤ 1,000,000 | O(n) needed | Two pointers, sliding window, prefix sum, hashmap |
| n ≤ 10^9 | O(log n) or O(1) | Binary search, math formula |
| String length ≤ 10^3 | O(n²) string DP | LCS, edit distance, palindrome |
| Graph: V,E ≤ 10^5 | O(V+E) or O(E log V) | BFS/DFS, Dijkstra |

**This table alone eliminates 50% of your thinking time.** Read constraints → know the algorithm family → confirm with signal words.

---

## Why Your Brain Resists DSA (And How to Fix It)

> Based on cognitive load theory, chunking research, and the psychology of expert performance.

### The 3 Invisible Walls

**Wall 1: Working Memory Overload**
Your brain holds only ~4 things in working memory at once. A DP problem asks you to track: the state definition, the transition, the base case, the loop order, AND the edge cases — that's 5+ things. Your brain literally cannot hold it all.

**Fix: Externalize.** Write the state, transition, and base case on paper BEFORE coding. Now your working memory only needs to handle implementation. This is why dry-running on paper works — it offloads memory to the paper.

**Wall 2: The Einstellung Effect (Your Experience Traps You)**
Once you've solved 10 problems with a HashMap, your brain will try HashMap on EVERYTHING — even when sliding window is the right answer. Your past success literally blocks you from seeing the better solution.

**Fix: Always ask yourself TWO questions before committing to an approach:**
1. "What pattern does my gut say?" (your first instinct)
2. "What if my gut is wrong — what ELSE could this be?" (force a second option)

Then compare both. This 10-second habit breaks the Einstellung trap.

**Wall 3: Functional Fixedness (You See Tools Too Narrowly)**
You think "stack = parentheses matching." So when you see "next greater element," you don't think of a stack — because that's not what stacks "do" in your mental model.

**Fix: For each data structure, learn its MULTIPLE uses:**
- Stack: parentheses matching AND monotonic stack AND undo operations AND DFS
- HashMap: lookup AND frequency counting AND prefix sum caching AND grouping
- Heap: top K AND merge K sorted AND median AND scheduling
- Binary Search: sorted array AND answer space AND first/last occurrence

---

## How to Make Patterns Stick Permanently

> Based on desirable difficulty research, spaced repetition, interleaving, and transfer-appropriate processing.

### The 5 Science-Backed Rules

**Rule 1: Interleave, Don't Block**
- ❌ Solve 10 sliding window problems in a row → you'll pattern-match by topic, not by problem
- ✅ Solve 1 sliding window, then 1 tree, then 1 DP, then 1 graph → your brain learns to IDENTIFY the pattern, not just apply it

After finishing each phase in this guide, go back and mix problems from all completed phases. This is harder but builds real pattern recognition.

**Rule 2: Sleep Between Hard Problems**
When you're stuck on a hard problem at night, stop. Sleep. Your brain consolidates and reorganizes information during sleep (the "incubation effect"). You'll often see the solution clearly the next morning. This isn't motivation advice — it's neuroscience.

**Rule 3: Practice Like You'll Perform**
If interviews are 45 minutes with someone watching → practice with a timer and explain out loud. Your brain encodes memories with context. If you always practice in silence with no timer, your recall will fail under interview pressure. This is called "transfer-appropriate processing."

**Rule 4: Struggle Is the Signal of Learning**
If a problem feels easy, you're not learning — you're reviewing. Real learning happens at the edge of your ability where you struggle but eventually succeed. The Codeforces community calls this "jumping gaps." If you're not failing ~40-50% of problems on first attempt, you're practicing too easy.

**Rule 5: Teach It to Lock It In**
After solving a problem, explain the solution out loud as if teaching someone. Use simple words. If you stumble or can't explain WHY a step works, you don't truly understand it. This is the Feynman technique — and it converts shallow understanding into deep, permanent knowledge.

### The "Reverse Editorial" Technique

> This is the single most powerful learning technique nobody talks about.

After reading an editorial/solution for a problem you couldn't solve:

1. **Don't memorize the solution.** Instead, ask: "What OBSERVATION would I need to make to arrive at this solution on my own?"
2. **Trace backwards:** Solution uses sliding window → what in the problem should have told me "sliding window"? → the word "contiguous" + "longest" + constraint n=10^5
3. **Write one line:** "Signal: contiguous + longest + large n → Sliding Window"
4. **Next time you see those signals, your brain will fire the connection automatically.**

This builds the same neural pathways that grandmasters have — you're just building them consciously instead of through thousands of problems.

---

## Phase 1: Arrays, Strings & Hashing (The Foundation)

> **Real-world connection:** Every API response you parse, every log file you filter,
> every database query result you process — it's all arrays and strings.
> Hashing is literally what your app's cache does.

### Pattern 1.1: HashMap / Frequency Counting

**Real life:** You count how many times each error code appears in your logs. That's a frequency map.

**When you see:** "find duplicates", "count occurrences", "group by", "two sum", "anagram"

**How it works:** Store values in a dictionary for O(1) lookup instead of scanning the whole array.

**Learn it:**
- 📺 [NeetCode — Two Sum](https://www.youtube.com/watch?v=KLlXCFG5TnA) (the most important problem in DSA)
- 📺 [NeetCode — Group Anagrams](https://www.youtube.com/watch?v=vzdNOK2oB2E)
- 🔗 [VisuAlgo — Hash Table](https://visualgo.net/en/hashtable)

**Practice (do in this order):**
1. [Two Sum](https://leetcode.com/problems/two-sum/) — if you solve ONE problem in your life, solve this
2. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
3. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
4. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
5. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

**Dry run Two Sum yourself before watching the solution.** Use the table method from Step 2.

---

### Pattern 1.2: Two Pointers

**Real life:** You're comparing items from both ends of a shelf to find a matching pair. Or you're removing duplicates from a sorted spreadsheet by scanning with two fingers.

**When you see:** sorted array, pair sum, palindrome, remove duplicates, merge sorted, "in-place"

**How it works:** Two pointers move toward each other (or in the same direction) to avoid nested loops. Turns O(n²) into O(n).

**Learn it:**
- 📺 [NeetCode — Two Pointers playlist](https://www.youtube.com/watch?v=cQ1Oz4ckceM&list=PLot-Xpze53leOBgcVsJBEGrHPd_7x_koV)
- 📺 [NeetCode — Container With Most Water](https://www.youtube.com/watch?v=UuiTKBwPgAo)

**Practice:**
1. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
2. [Two Sum II (sorted)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
3. [3Sum](https://leetcode.com/problems/3sum/) — sort + two pointers, classic
4. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
5. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) — the boss fight

---

### Pattern 1.3: Sliding Window

**Real life:** You're monitoring server CPU usage over a rolling 5-minute window. Or scanning through a string of user input looking for the longest valid session.

**When you see:** "contiguous subarray", "substring", "window of size K", "longest", "shortest", "at most K"

**How it works:** Maintain a window [left, right]. Expand right to include more. Shrink left when the window breaks a condition. Never go backwards.

**The template (memorize this):**
```
left = 0
for right in range(len(arr)):
    # add arr[right] to window state
    while window_is_invalid():
        # remove arr[left] from window state
        left += 1
    # update answer
```

**Learn it:**
- 📺 [NeetCode — Sliding Window playlist](https://www.youtube.com/watch?v=1pkOgXD63yU&list=PLot-Xpze53leOBgcVsJBEGrHPd_7x_koV)
- 📺 [takeUforward — Sliding Window & Two Pointer](https://www.youtube.com/watch?v=-ZTlMn8sSEQ)
- 🔗 [AlgoMaster — Sliding Window](https://blog.algomaster.io/p/15-leetcode-patterns)

**Practice:**
1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
2. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
3. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) — hard but essential
4. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) — uses deque

---

### Pattern 1.4: Prefix Sum

**Real life:** Your bank statement shows a running balance. To find how much you spent between Tuesday and Friday, you subtract Tuesday's balance from Friday's. That's prefix sum.

**When you see:** "subarray sum equals K", "range sum", "count subarrays with sum"

**The template:**
```
# Build prefix sum
prefix = [0] * (len(arr) + 1)
for i in range(len(arr)):
    prefix[i+1] = prefix[i] + arr[i]

# Sum of arr[i..j] = prefix[j+1] - prefix[i]
```

**For "count subarrays with sum K" — combine with hashmap:**
```
count, curr_sum = 0, 0
seen = {0: 1}  # prefix sum → how many times we've seen it
for num in arr:
    curr_sum += num
    count += seen.get(curr_sum - k, 0)
    seen[curr_sum] = seen.get(curr_sum, 0) + 1
```

**Learn it:**
- 📺 [NeetCode — Subarray Sum Equals K](https://www.youtube.com/watch?v=fFVZt-6sgyo)

**Practice:**
1. [Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/) — warm up
2. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) — prefix sum + hashmap
3. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

---

### Pattern 1.5: Kadane's Algorithm

**Real life:** You're looking at daily profit/loss and want to find the best consecutive streak.

**When you see:** "maximum subarray sum", "maximum product"

**The idea:** At each position, decide: extend the current streak or start fresh?

**The template:**
```
max_sum = curr_sum = arr[0]
for num in arr[1:]:
    curr_sum = max(num, curr_sum + num)  # extend or restart?
    max_sum = max(max_sum, curr_sum)
```

**Practice:**
1. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) — THE Kadane's problem
2. [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

---

### Pattern 1.6: Matrix

**Real life:** Spreadsheets, game boards, image pixels — all 2D arrays.

**Practice:**
1. [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
2. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
3. [Rotate Image](https://leetcode.com/problems/rotate-image/)

> ✅ **Phase 1 checkpoint:** You should be able to look at any array/string problem and say
> "this is a [two pointer / sliding window / prefix sum / hashmap] problem" within 60 seconds.
> If not, redo the problems above. Don't move on.

### 🎤 Phase 1 — Interviewer FAQ (Expect These Follow-Ups)

**After you solve any array/string problem, the interviewer WILL ask:**

- "Can you do it in-place (O(1) space)?" → This tests if you can avoid creating a new array. Two pointers often enables this.
- "What if the array is unsorted?" → Your sorted-array solution won't work. Think hashmap.
- "What if there are duplicates?" → 3Sum and sliding window problems break if you don't skip duplicates. Always ask upfront.
- "What's the time and space complexity?" → You MUST answer this instantly. Practice stating it before you code.
- "Can you handle negative numbers?" → Prefix sum and Kadane's behave differently with negatives. Maximum product subarray needs to track both min and max.
- "What if the input is empty or has one element?" → Your code should handle this without crashing. Check before the loop.

**What top programmers see that you don't:**
- When they see "subarray" → they immediately think: sliding window OR prefix sum. Never nested loops.
- When they see "sorted" → they immediately think: binary search OR two pointers. Never linear scan.
- When they see "frequency" or "count" → hashmap. Always.
- When they see "contiguous" → sliding window. The word "contiguous" is the trigger.

---

## Phase 2: Binary Search & Stacks

### Pattern 2.1: Binary Search

**Real life:** You're looking up a word in a dictionary. You don't start from page 1 — you open to the middle, then go left or right. That's binary search.

**When you see:** "sorted", "find target", "first/last occurrence", "peak", "rotated sorted array"

**The template:**
```
lo, hi = 0, len(arr) - 1
while lo <= hi:
    mid = lo + (hi - lo) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: lo = mid + 1
    else: hi = mid - 1
```

**Learn it:**
- 📺 [NeetCode — Binary Search playlist](https://www.youtube.com/watch?v=s4DPM8ct1pI&list=PLot-Xpze53leNZQd0iINpD-MAhMOMzWvO)
- 🔗 [VisuAlgo — Binary Search](https://visualgo.net/en/bst)

**Practice:**
1. [Binary Search](https://leetcode.com/problems/binary-search/) — the basics
2. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
3. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
4. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

### Binary Search on Answer (This is the real power move)

**Real life:** "What's the minimum truck capacity to ship all packages in D days?" You don't try every capacity — you binary search on the answer space.

**When you see:** "minimize the maximum", "maximize the minimum", "minimum speed/capacity/days"

**The template:**
```
lo, hi = min_possible_answer, max_possible_answer
while lo <= hi:
    mid = lo + (hi - lo) // 2
    if can_achieve(mid):  # write a helper that checks if 'mid' is feasible
        ans = mid
        hi = mid - 1      # try smaller (for minimize problems)
    else:
        lo = mid + 1
```

**Practice:**
1. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) — start here
2. [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)
3. [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)
4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) — hard classic

---

### Pattern 2.2: Stacks

**Real life:** Undo/redo in any app. Browser back button. Matching opening and closing brackets in your IDE.

**When you see:** "valid parentheses", "next greater element", "decode string", "largest rectangle"

**Learn it:**
- 📺 [NeetCode — Stack playlist](https://www.youtube.com/watch?v=WTzjTskDFMg&list=PLot-Xpze53lfxD6l5pAGvCD4nPvWKU8Qo)
- 🔗 [VisuAlgo — Stack](https://visualgo.net/en/list)

**Monotonic Stack** (the pattern interviewers love):
- Maintain a stack that's always increasing or always decreasing
- Solves "next greater/smaller element" in O(n)

**The template (next greater element — decreasing stack):**
```
result = [-1] * len(arr)
stack = []  # stores indices
for i in range(len(arr)):
    while stack and arr[i] > arr[stack[-1]]:
        result[stack.pop()] = arr[i]  # arr[i] is the next greater for popped element
    stack.append(i)
```

**Practice:**
1. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
2. [Min Stack](https://leetcode.com/problems/min-stack/)
3. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) — monotonic stack intro
4. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) — classic hard
5. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

> ✅ **Phase 2 checkpoint:** Binary search should feel like second nature on sorted data.
> You should be able to identify monotonic stack problems by the phrase "next greater/smaller."

### 🎤 Phase 2 — Interviewer FAQ

- "Your binary search has an off-by-one error. Can you fix it?" → The #1 bug in binary search. Always dry run with a 2-element array to catch it. Check: should it be `lo <= hi` or `lo < hi`? Should mid go to `lo` or `lo+1`?
- "What if the array has duplicates? Find the FIRST occurrence." → Standard binary search finds ANY occurrence. For first/last, don't return when you find target — keep searching left/right.
- "Can you solve this without extra space?" → For stack problems, ask yourself: can two pointers replace the stack? (Trapping Rain Water: yes. Valid Parentheses: no.)
- "What's the search space for binary search on answer?" → You must define `lo` and `hi` clearly. For Koko Eating Bananas: lo=1, hi=max(piles). Always explain your bounds.

**What top programmers see that you don't:**
- Binary search isn't just for sorted arrays. Any problem with a **monotonic condition** (if X works, then X+1 also works) can use binary search on the answer.
- Monotonic stack: when you see "for each element, find the nearest larger/smaller element" → that's O(n) with a stack, not O(n²) with nested loops.


---

## Phase 3: Linked Lists & Recursion

### Pattern 3.1: Linked Lists

**Real life:** A playlist where each song points to the next one. You can't jump to song #47 directly — you have to follow the chain. That's a linked list.

> Linked lists are declining in interview frequency, but Meta and Microsoft still ask them.
> LRU Cache (linked list + hashmap) is one of the most asked problems across all companies.

**The trick:** Always draw the pointers on paper before coding. Pointer manipulation is where bugs hide.

**Learn it:**
- 📺 [NeetCode — Linked List playlist](https://www.youtube.com/watch?v=G0_I-ZF0S38&list=PLot-Xpze53leU0Ec0VkBhnf4npMRFiNcB)
- 🔗 [VisuAlgo — Linked List](https://visualgo.net/en/list)

**Key patterns:**
- **Fast & Slow pointer:** Detect cycle, find middle, check palindrome
- **Reversal:** Reverse entire list, reverse in groups of K
- **Merge:** Merge two sorted lists, merge K sorted lists (uses heap)

**Practice:**
1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) — if you can't do this in your sleep, keep practicing
2. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
3. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
4. [Reorder List](https://leetcode.com/problems/reorder-list/) — combines find middle + reverse + merge
5. [LRU Cache](https://leetcode.com/problems/lru-cache/) — **top 5 most asked problem at Meta**
6. [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

---

### Pattern 3.2: Recursion & Backtracking

**Real life:** You're exploring a maze. At every fork, you pick a path. If it's a dead end, you backtrack and try the next path. That's recursion + backtracking.

Or think of it as: **recursion = delegation.** "I'll handle this one item, and delegate the rest to a smaller version of myself."

**The mental model:**
```
function solve(problem):
    if problem is tiny enough:     ← base case
        return answer directly
    
    for each choice:
        make the choice
        solve(smaller problem)     ← recursive call (trust it works)
        undo the choice            ← backtrack
```

**Learn it:**
- 📺 [Abdul Bari — Recursion](https://www.youtube.com/watch?v=kHi1DUhp9kM) — best conceptual explanation
- 📺 [NeetCode — Backtracking playlist](https://www.youtube.com/watch?v=REOH22Xwdkk&list=PLot-Xpze53lf5C3HSjCnyFghlW0G1HHXo)
- 🔗 [Recursion Visualizer](https://recursion.vercel.app/) — see the call stack in action

**Practice (in this exact order — each builds on the previous):**
1. [Subsets](https://leetcode.com/problems/subsets/) — the foundation of all backtracking
2. [Subsets II](https://leetcode.com/problems/subsets-ii/) — handle duplicates
3. [Combination Sum](https://leetcode.com/problems/combination-sum/)
4. [Permutations](https://leetcode.com/problems/permutations/)
5. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) — **very high frequency**
6. [Word Search](https://leetcode.com/problems/word-search/) — grid backtracking
7. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
8. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

> **Key insight:** If you see overlapping subproblems in your backtracking tree → it's actually a DP problem. We'll cover that in Phase 6.

> ✅ **Phase 3 checkpoint:** You should be able to write the backtracking template from memory.
> You should understand that recursion = "solve for one, delegate the rest."

### 🎤 Phase 3 — Interviewer FAQ

**Linked Lists:**
- "Can you do it without extra space?" → In-place reversal, fast/slow pointer. Draw the pointers.
- "What if there's a cycle?" → Floyd's algorithm. Follow-up: "Find where the cycle starts." (Reset one pointer to head, move both at speed 1.)
- "What's the time complexity of your LRU Cache operations?" → Must be O(1) for both get and put. That's why it's doubly-linked list + hashmap, not just a list.

**Backtracking:**
- "How do you avoid duplicate combinations?" → Sort the input first, then skip `if i > start and nums[i] == nums[i-1]`.
- "What's the time complexity?" → For subsets: O(2ⁿ). For permutations: O(n!). Know these by heart.
- "Can you prune earlier?" → Always look for ways to stop recursion early. If remaining sum < 0, return immediately.

**What top programmers see that you don't:**
- Every backtracking problem has the same skeleton: choose → explore → unchoose. The only thing that changes is what "choose" means.
- If your backtracking solution is TLE (time limit exceeded), check: are there overlapping subproblems? If yes → it's DP, not backtracking.

---

## Phase 4: Trees

> **Real life:** Your company's org chart is a tree. The file system on your computer is a tree.
> The DOM in your web app is a tree. JSON is a tree. Trees are everywhere.

**Why trees are asked so much:** They test recursion, traversal, and your ability to think in terms of subproblems — all at once.

**Learn it:**
- 📺 [NeetCode — Trees playlist](https://www.youtube.com/watch?v=OnSn2XEQ4MY&list=PLot-Xpze53ldg4pN6PfzoJY7KsKcxF1jg)
- 🔗 [VisuAlgo — Binary Search Tree](https://visualgo.net/en/bst)
- 🔗 [VisuAlgo — Binary Heap](https://visualgo.net/en/heap)

### Pattern 4.1: DFS on Trees (Depth-First)

**Real life:** You're exploring a folder structure — you go deep into one folder before coming back up.

**The template (most tree problems are a variation of this):**
```
def dfs(node):
    if not node: return base_case

    left = dfs(node.left)     # trust this gives the right answer for left subtree
    right = dfs(node.right)   # trust this gives the right answer for right subtree

    # combine left, right, and node.val to get answer for this subtree
    return combine(left, right, node.val)
```

**Three traversals (know all three):**
- **Inorder** (left → root → right): gives sorted order for BST
- **Preorder** (root → left → right): used for serialization
- **Postorder** (left → right → root): used for bottom-up computation (like calculating folder sizes)

**Practice:**
1. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) — simplest tree recursion
2. [Same Tree](https://leetcode.com/problems/same-tree/)
3. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
4. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
5. [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
6. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

### Pattern 4.2: BFS on Trees (Level-Order)

**Real life:** You're sending a company-wide announcement — it goes to the CEO first, then VPs, then directors, then managers... level by level. That's BFS.

**The template:**
```
from collections import deque
queue = deque([root])
while queue:
    for _ in range(len(queue)):   # process one level at a time
        node = queue.popleft()
        # do something with node
        if node.left:  queue.append(node.left)
        if node.right: queue.append(node.right)
```

**Practice:**
1. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
2. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
3. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

### Pattern 4.3: Path & Ancestor Problems

**Practice:**
1. [Path Sum](https://leetcode.com/problems/path-sum/)
2. [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) — **top 5 most asked tree problem**
3. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) — hard classic

### Pattern 4.4: BST-Specific

**The key property:** Everything left < root < everything right. This means inorder traversal = sorted order.

**Practice:**
1. [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) — pass min/max range down
2. [Kth Smallest Element in BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
3. [Convert Sorted Array to BST](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### Pattern 4.5: Construction & Serialization

**Practice:**
1. [Construct Binary Tree from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
2. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) — **Amazon favorite**

> ✅ **Phase 4 checkpoint:** You should be able to solve any tree problem by asking:
> "Do I need DFS (go deep) or BFS (go wide)? Top-down or bottom-up?"

### 🎤 Phase 4 — Interviewer FAQ

- "Can you do it iteratively instead of recursively?" → Every DFS can be converted to iterative using an explicit stack. Every BFS already uses a queue. Practice iterative inorder traversal.
- "What if the tree is not balanced? What's the worst case?" → Skewed tree = linked list. Depth becomes O(n) instead of O(log n). Mention this.
- "How would you handle a very large tree that doesn't fit in memory?" → BFS (level-order) is better because it only holds one level in memory at a time. DFS holds the entire path from root to current node.
- "What's the difference between path sum and maximum path sum?" → Path sum: root to leaf. Maximum path sum: ANY node to ANY node (can go through root). The latter is much harder — you need to track "max gain from left" and "max gain from right" at each node.
- "Is this a valid BST?" → The trap: checking `node.left < node < node.right` is NOT enough. You need to pass a valid RANGE (min, max) down the tree.

**What top programmers see that you don't:**
- Most tree problems are either "top-down" (pass info from parent to child via parameters) or "bottom-up" (return info from child to parent via return values). Identify which one BEFORE coding.
- If the problem says "binary search tree" → inorder traversal gives sorted order. This unlocks many shortcuts.

---

## Phase 5: Heaps, Greedy & Intervals

### Pattern 5.1: Heaps / Priority Queues

**Real life:** A hospital ER — patients aren't seen in arrival order, they're seen by severity. That's a priority queue (heap).

**When you see:** "top K", "kth largest/smallest", "merge K sorted", "median from stream", "schedule"

**Learn it:**
- 📺 [NeetCode — Heap playlist](https://www.youtube.com/watch?v=t0Cq6tVNRBA&list=PLot-Xpze53lfOSoGKgBdMHPVwC1iYkZGM)
- 🔗 [VisuAlgo — Binary Heap](https://visualgo.net/en/heap)

**Practice:**
1. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
3. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) — **Meta classic**
4. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) — two heaps pattern
5. [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
6. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

### Pattern 5.2: Greedy

**Real life:** At a buffet, you always pick the dish that looks best right now. If that local choice always leads to the best overall meal — that's greedy.

**When you see:** "minimum number of", "maximum number of", "can you reach", "schedule", "assign"

**The test:** Can I prove that the locally optimal choice is always globally optimal? If yes → greedy. If no → probably DP.

**Practice:**
1. [Jump Game](https://leetcode.com/problems/jump-game/)
2. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)
3. [Gas Station](https://leetcode.com/problems/gas-station/)
4. [Candy](https://leetcode.com/problems/candy/)
5. [Hand of Straights](https://leetcode.com/problems/hand-of-straights/)

### Pattern 5.3: Intervals

**Real life:** Scheduling meeting rooms. "Can I attend all meetings?" "What's the minimum number of rooms I need?"

**The trick:** Almost always sort by start time first. Then scan left to right.

**Practice:**
1. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
2. [Insert Interval](https://leetcode.com/problems/insert-interval/)
3. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
4. [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) (if you have LC premium, otherwise [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/))
5. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

> ✅ **Phase 5 checkpoint:** You should know when to use a heap vs sorting vs greedy.
> Intervals should feel mechanical — sort, scan, merge/count.

### 🎤 Phase 5 — Interviewer FAQ

**Heaps:**
- "Why use a heap instead of sorting?" → Heap gives you top K in O(n log k). Sorting is O(n log n). When k << n, heap wins.
- "Min-heap or max-heap?" → For "kth largest": use a MIN-heap of size k. The top of the heap IS the kth largest. This confuses most candidates.
- "How does Find Median from Data Stream work?" → Two heaps: max-heap for the left half, min-heap for the right half. Balance their sizes. Median is the top of one or average of both tops.

**Greedy:**
- "How do you PROVE your greedy approach is correct?" → Exchange argument: "If I swap my greedy choice with any other choice, the result doesn't improve." You don't need a formal proof in interviews, but you need to articulate WHY greedy works.
- "What if greedy doesn't work here?" → Fall back to DP. Jump Game I is greedy. Coin Change is NOT greedy (greedy fails for coins [1, 3, 4] with target 6).

**Intervals:**
- "Sort by start time or end time?" → Merge intervals: sort by start. Activity selection / non-overlapping: sort by end. Know which is which.
- "What if intervals are given as a stream?" → You can't sort a stream. Use a heap (Meeting Rooms II pattern).


---

## Phase 6: Graphs (The Big One)

> **Real life:** Social networks (who follows whom), Google Maps (cities and roads),
> your microservices architecture (service A calls service B), package dependencies
> (npm install), course prerequisites. **Graphs are everywhere in real systems.**
>
> ~35-40% of coding interviews at major tech companies include at least one graph problem.

**Learn it:**
- 📺 [NeetCode — Graphs playlist](https://www.youtube.com/watch?v=EgI5nU9etnU&list=PLot-Xpze53ldBT_7QA8NVot219jFNr_GI)
- 📺 [William Fiset — Graph Theory playlist](https://www.youtube.com/watch?v=DgXR2OWQnLc&list=PLDV1Zeh2NRsDGO4--qE8yH72HFL1Km93P) — the most comprehensive free resource
- 🔗 [VisuAlgo — Graph Traversal](https://visualgo.net/en/dfsbfs)

### Pattern 6.1: BFS (Breadth-First Search)

**Real life:** Finding the shortest route between two subway stations. You explore all stations 1 stop away, then 2 stops away, etc.

**When you see:** "shortest path" (unweighted), "minimum steps", "level by level", "nearest"

**The template:**
```
queue = [start]
visited = {start}
while queue:
    node = queue.pop(0)  # or use deque.popleft()
    for neighbor in graph[node]:
        if neighbor not in visited:
            visited.add(neighbor)
            queue.append(neighbor)
```

**Practice:**
1. [Number of Islands](https://leetcode.com/problems/number-of-islands/) — **THE most asked graph problem**
2. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) — multi-source BFS
3. [Word Ladder](https://leetcode.com/problems/word-ladder/)
4. [01 Matrix](https://leetcode.com/problems/01-matrix/)
5. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)

### Pattern 6.2: DFS (Depth-First Search)

**Real life:** Exploring a cave system — you go as deep as possible down one tunnel before backtracking.

**When you see:** "all paths", "connected components", "detect cycle", "flood fill"

**Practice:**
1. [Flood Fill](https://leetcode.com/problems/flood-fill/)
2. [Clone Graph](https://leetcode.com/problems/clone-graph/)
3. [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
4. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
5. [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

### Pattern 6.3: Topological Sort

**Real life:** You can't deploy Service B before Service A is running, and Service A needs the database first. Topological sort gives you the correct deployment order.

**When you see:** "prerequisites", "dependencies", "ordering", "schedule"

**The approach:** Kahn's algorithm (BFS with indegree counting) — preferred in interviews.

**The template:**
```
from collections import deque
indegree = [0] * n
for u, v in edges: indegree[v] += 1

queue = deque([i for i in range(n) if indegree[i] == 0])
order = []
while queue:
    node = queue.popleft()
    order.append(node)
    for neighbor in graph[node]:
        indegree[neighbor] -= 1
        if indegree[neighbor] == 0:
            queue.append(neighbor)

has_cycle = len(order) != n  # if true, topological sort is impossible
```

**Practice:**
1. [Course Schedule](https://leetcode.com/problems/course-schedule/) — **Amazon/Google classic**
2. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) — return the order
3. [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)

### Pattern 6.4: Union-Find (Disjoint Set Union)

**Real life:** "Are these two users in the same friend group?" Union-Find answers this in near O(1) time.

**When you see:** "connected components", "redundant connection", "merge groups", "accounts merge"

**The template (memorize this — it's tiny):**
```
parent = list(range(n))
rank = [0] * n

def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])  # path compression
    return parent[x]

def union(x, y):
    px, py = find(x), find(y)
    if px == py: return False
    if rank[px] < rank[py]: px, py = py, px
    parent[py] = px
    if rank[px] == rank[py]: rank[px] += 1
    return True
```

**Practice:**
1. [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
2. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
3. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

### Pattern 6.5: Shortest Path (Weighted Graphs)

**Real life:** Google Maps finding the fastest route considering traffic (weights on edges).

**Know Dijkstra's — that's enough for interviews:**

**The template:**
```
import heapq
dist = {node: float('inf') for node in graph}
dist[start] = 0
heap = [(0, start)]  # (distance, node)

while heap:
    d, u = heapq.heappop(heap)
    if d > dist[u]: continue  # skip outdated entries
    for v, weight in graph[u]:
        if dist[u] + weight < dist[v]:
            dist[v] = dist[u] + weight
            heapq.heappush(heap, (dist[v], v))
```
- 📺 [NeetCode — Network Delay Time](https://www.youtube.com/watch?v=EaphyqKU4PQ)
- 🔗 [VisuAlgo — Dijkstra](https://visualgo.net/en/sssp)

**Practice:**
1. [Network Delay Time](https://leetcode.com/problems/network-delay-time/) — pure Dijkstra
2. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) — modified BFS/Bellman-Ford
3. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

> ✅ **Phase 6 checkpoint:** You should be able to model any "connected things" problem as a graph
> and immediately know: BFS for shortest path, DFS for exploration, topo sort for ordering,
> union-find for grouping, Dijkstra for weighted shortest path.

### 🎤 Phase 6 — Interviewer FAQ

- "BFS or DFS? Why?" → BFS = shortest path in unweighted graph, level-order. DFS = explore all paths, detect cycles, connected components. If the interviewer asks "shortest" → BFS. If "all paths" or "exists" → DFS.
- "How do you detect a cycle?" → Undirected: if you visit a node that's already visited AND it's not the parent → cycle. Directed: use 3 colors (white/gray/black) or a recursion stack.
- "What if the graph has negative weights?" → Dijkstra breaks. Use Bellman-Ford. But in interviews, 95% of the time weights are non-negative.
- "Can you do it without recursion?" → DFS with an explicit stack. BFS already uses a queue. Topological sort with Kahn's (BFS) avoids recursion entirely.
- "What's the difference between Union-Find and DFS for connected components?" → Same result, different tradeoffs. Union-Find is better for dynamic graphs (edges added over time). DFS is simpler for static graphs.
- "How do you represent the graph?" → Adjacency list (hashmap of lists) for sparse graphs. Adjacency matrix for dense graphs. In interviews, almost always use adjacency list.

**What top programmers see that you don't:**
- Many problems are "hidden graphs." Word Ladder: each word is a node, words differing by one letter are connected. Course Schedule: courses are nodes, prerequisites are edges.
- If the problem says "minimum" + "unweighted" → BFS. If "minimum" + "weighted" → Dijkstra. This decision takes 2 seconds.

---

## Phase 7: Dynamic Programming (The Final Boss)

> **Real life:** You're calculating shipping costs for different package combinations.
> Instead of recalculating from scratch each time, you store results of sub-calculations
> and reuse them. That's DP — **remembering past work to avoid redoing it.**
>
> DP is just: **recursion + memory.**

**The mindset shift:** Don't think "what DP table do I need?" Think:
1. Can I solve this recursively?
2. Am I solving the same subproblem multiple times?
3. If yes → store the results. That's it. That's DP.

**The memoization template (use this in interviews — it's easier to write):**
```
from functools import lru_cache

@lru_cache(maxsize=None)
def dp(state):
    if base_case: return base_value

    result = initial_value
    for choice in choices:
        result = best(result, dp(next_state))
    return result
```

**The tabulation template (bottom-up — offer this if interviewer asks):**
```
dp = [base_values]
for i in range(start, end):
    for choice in choices:
        dp[i] = best(dp[i], dp[previous_state] + cost)
return dp[final_state]
```

**Learn it:**
- 📺 [NeetCode — DP playlist](https://www.youtube.com/watch?v=73r3KWiEvyk&list=PLot-Xpze53lcvx_yhUmHBOLoRQ0czSfAo) — best structured playlist
- 📺 [Abdul Bari — Dynamic Programming](https://www.youtube.com/watch?v=5dRGRueKU3M) — conceptual foundation
- 📺 [AlgoMaster — 20 DP Patterns](https://blog.algomaster.io/p/20-patterns-to-master-dynamic-programming)
- 🔗 [VisuAlgo — DP (Fibonacci, LIS, etc.)](https://visualgo.net/en)

### Pattern 7.1: 1D DP (Start Here)

**Real life:** "How many ways can I climb to the 10th floor if I can take 1 or 2 steps at a time?" Each floor's answer depends on the previous floors.

**Practice (do in this exact order):**
1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) — fibonacci in disguise
2. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
3. [House Robber](https://leetcode.com/problems/house-robber/) — take/skip pattern
4. [House Robber II](https://leetcode.com/problems/house-robber-ii/) — circular variant
5. [Decode Ways](https://leetcode.com/problems/decode-ways/)
6. [Coin Change](https://leetcode.com/problems/coin-change/) — **classic**
7. [Coin Change II](https://leetcode.com/problems/coin-change-ii/) — count ways
8. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
9. [Word Break](https://leetcode.com/problems/word-break/)

### Pattern 7.2: 2D / Grid DP

**Real life:** "How many unique routes can a delivery robot take from warehouse (top-left) to customer (bottom-right) on a grid?"

**Practice:**
1. [Unique Paths](https://leetcode.com/problems/unique-paths/)
2. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/) — with obstacles
3. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
4. [Maximal Square](https://leetcode.com/problems/maximal-square/)

### Pattern 7.3: String DP (High Frequency)

**Real life:** Spell checkers use edit distance to suggest corrections. Diff tools use LCS to show file changes.

**Practice:**
1. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
2. [Edit Distance](https://leetcode.com/problems/edit-distance/) — **Google/Amazon classic**
3. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
4. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

### Pattern 7.4: Knapsack

**Real life:** You have a backpack with limited capacity. Each item has a weight and value. Maximize value without exceeding capacity.

**Practice:**
1. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) — 0/1 knapsack in disguise
2. [Target Sum](https://leetcode.com/problems/target-sum/)

### Pattern 7.5: Stock Problems (State Machine DP)

**Practice:**
1. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) — one transaction
2. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) — unlimited
3. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

> ✅ **Phase 7 checkpoint:** For any DP problem, you should be able to:
> 1. Write the brute force recursion
> 2. Identify overlapping subproblems
> 3. Add memoization
> 4. (Optional) Convert to bottom-up tabulation

### 🎤 Phase 7 — Interviewer FAQ

- "What's the state? What's the transition?" → This is THE question. State = "what information do I need to make a decision at step i?" Transition = "how does dp[i] relate to previous states?" If you can answer these two, you can solve any DP problem.
- "Can you optimize the space?" → Most 2D DP only depends on the previous row → reduce to two 1D arrays. Most 1D DP only depends on the previous 1-2 values → reduce to variables.
- "Top-down or bottom-up?" → Top-down (memoization) is easier to write and think about. Bottom-up (tabulation) is faster (no recursion overhead). Start with top-down in interviews, offer to convert if asked.
- "Is this greedy or DP?" → If the locally optimal choice is always globally optimal → greedy. If you need to consider ALL choices to find the best → DP. Coin Change with coins [1,3,4], target 6: greedy picks 4+1+1=3 coins, but optimal is 3+3=2 coins. Greedy fails → use DP.
- "What if the string is very long?" → String DP is usually O(n²) or O(n*m). For n > 10⁴, you might need a different approach (binary search + hashing for LIS, or suffix arrays).

**What top programmers see that you don't:**
- DP problems always start as recursion. If you can't write the recursive solution, you can't write the DP solution. Always start with recursion.
- The hardest part of DP is defining the state. Once you have the state, the transition usually follows naturally. Spend 80% of your thinking time on "what is my state?"
- Common DP state patterns: `dp[i]` = answer for first i elements. `dp[i][j]` = answer for substring i..j. `dp[i][w]` = answer using first i items with capacity w (knapsack).

---

## Phase 8: Tries & Bit Manipulation (Quick Wins)

### Pattern 8.1: Tries

**Real life:** Autocomplete in your search bar. Every keystroke narrows down suggestions by prefix. That's a trie.

**Only learn if targeting Google or have extra time:**
1. [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)
2. [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
3. [Word Search II](https://leetcode.com/problems/word-search-ii/)

### Pattern 8.2: Bit Manipulation

**Real life:** Feature flags in your app (each bit = a feature on/off). Permission systems (read/write/execute = 3 bits).

**Just know these tricks:**
- `a ^ a = 0` (XOR cancels duplicates)
- `n & (n-1)` removes lowest set bit → check power of 2
- `n & (1 << i)` checks if bit i is set

**Practice:**
1. [Single Number](https://leetcode.com/problems/single-number/)
2. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
3. [Counting Bits](https://leetcode.com/problems/counting-bits/)
4. [Missing Number](https://leetcode.com/problems/missing-number/)

---

## The 50 Problems You Cannot Skip

> These are the highest-frequency problems across FAANG (2020-2025).
> Sorted by phase so you encounter them naturally as you follow this guide.

| # | Problem | Pattern | Link |
|---|---|---|---|
| 1 | Two Sum | HashMap | [LC #1](https://leetcode.com/problems/two-sum/) |
| 2 | 3Sum | Two Pointers | [LC #15](https://leetcode.com/problems/3sum/) |
| 3 | Product of Array Except Self | Prefix | [LC #238](https://leetcode.com/problems/product-of-array-except-self/) |
| 4 | Maximum Subarray | Kadane's | [LC #53](https://leetcode.com/problems/maximum-subarray/) |
| 5 | Container With Most Water | Two Pointers | [LC #11](https://leetcode.com/problems/container-with-most-water/) |
| 6 | Trapping Rain Water | Stack/Two Ptr | [LC #42](https://leetcode.com/problems/trapping-rain-water/) |
| 7 | Longest Substring Without Repeating | Sliding Window | [LC #3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| 8 | Minimum Window Substring | Sliding Window | [LC #76](https://leetcode.com/problems/minimum-window-substring/) |
| 9 | Group Anagrams | HashMap | [LC #49](https://leetcode.com/problems/group-anagrams/) |
| 10 | Subarray Sum Equals K | Prefix + Hash | [LC #560](https://leetcode.com/problems/subarray-sum-equals-k/) |
| 11 | Merge Intervals | Intervals | [LC #56](https://leetcode.com/problems/merge-intervals/) |
| 12 | Search in Rotated Sorted Array | Binary Search | [LC #33](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| 13 | Koko Eating Bananas | BS on Answer | [LC #875](https://leetcode.com/problems/koko-eating-bananas/) |
| 14 | Median of Two Sorted Arrays | Binary Search | [LC #4](https://leetcode.com/problems/median-of-two-sorted-arrays/) |
| 15 | Valid Parentheses | Stack | [LC #20](https://leetcode.com/problems/valid-parentheses/) |
| 16 | Largest Rectangle in Histogram | Mono Stack | [LC #84](https://leetcode.com/problems/largest-rectangle-in-histogram/) |
| 17 | Daily Temperatures | Mono Stack | [LC #739](https://leetcode.com/problems/daily-temperatures/) |
| 18 | Reverse Linked List | Linked List | [LC #206](https://leetcode.com/problems/reverse-linked-list/) |
| 19 | LRU Cache | LL + HashMap | [LC #146](https://leetcode.com/problems/lru-cache/) |
| 20 | Merge K Sorted Lists | Heap + LL | [LC #23](https://leetcode.com/problems/merge-k-sorted-lists/) |
| 21 | Generate Parentheses | Backtracking | [LC #22](https://leetcode.com/problems/generate-parentheses/) |
| 22 | Combination Sum | Backtracking | [LC #39](https://leetcode.com/problems/combination-sum/) |
| 23 | Word Search | Backtracking | [LC #79](https://leetcode.com/problems/word-search/) |
| 24 | Subsets | Backtracking | [LC #78](https://leetcode.com/problems/subsets/) |
| 25 | Maximum Depth of Binary Tree | Tree DFS | [LC #104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| 26 | Lowest Common Ancestor | Tree DFS | [LC #236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) |
| 27 | Level Order Traversal | Tree BFS | [LC #102](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| 28 | Validate BST | Tree DFS | [LC #98](https://leetcode.com/problems/validate-binary-search-tree/) |
| 29 | Serialize/Deserialize Binary Tree | Tree | [LC #297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) |
| 30 | Binary Tree Maximum Path Sum | Tree DFS | [LC #124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |
| 31 | Kth Smallest in BST | Tree DFS | [LC #230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) |
| 32 | Top K Frequent Elements | Heap | [LC #347](https://leetcode.com/problems/top-k-frequent-elements/) |
| 33 | K Closest Points to Origin | Heap | [LC #973](https://leetcode.com/problems/k-closest-points-to-origin/) |
| 34 | Find Median from Data Stream | Two Heaps | [LC #295](https://leetcode.com/problems/find-median-from-data-stream/) |
| 35 | Number of Islands | Graph BFS/DFS | [LC #200](https://leetcode.com/problems/number-of-islands/) |
| 36 | Course Schedule | Topo Sort | [LC #207](https://leetcode.com/problems/course-schedule/) |
| 37 | Rotting Oranges | Multi-BFS | [LC #994](https://leetcode.com/problems/rotting-oranges/) |
| 38 | Word Ladder | Graph BFS | [LC #127](https://leetcode.com/problems/word-ladder/) |
| 39 | Clone Graph | Graph DFS | [LC #133](https://leetcode.com/problems/clone-graph/) |
| 40 | Pacific Atlantic Water Flow | Graph DFS | [LC #417](https://leetcode.com/problems/pacific-atlantic-water-flow/) |
| 41 | Network Delay Time | Dijkstra | [LC #743](https://leetcode.com/problems/network-delay-time/) |
| 42 | Climbing Stairs | 1D DP | [LC #70](https://leetcode.com/problems/climbing-stairs/) |
| 43 | House Robber | 1D DP | [LC #198](https://leetcode.com/problems/house-robber/) |
| 44 | Coin Change | 1D DP | [LC #322](https://leetcode.com/problems/coin-change/) |
| 45 | Longest Increasing Subsequence | 1D DP | [LC #300](https://leetcode.com/problems/longest-increasing-subsequence/) |
| 46 | Word Break | 1D DP | [LC #139](https://leetcode.com/problems/word-break/) |
| 47 | Edit Distance | String DP | [LC #72](https://leetcode.com/problems/edit-distance/) |
| 48 | Longest Palindromic Substring | String DP | [LC #5](https://leetcode.com/problems/longest-palindromic-substring/) |
| 49 | Unique Paths | Grid DP | [LC #62](https://leetcode.com/problems/unique-paths/) |
| 50 | Partition Equal Subset Sum | Knapsack | [LC #416](https://leetcode.com/problems/partition-equal-subset-sum/) |

---

## How to Build a Lasting Mental Model (The Meta-Skill)

> The science-backed learning rules are already covered in "How to Make Patterns Stick Permanently"
> above. This section covers the remaining practical techniques.

### When You're Stuck (The 20-Minute Protocol)

Most people stare at the screen when stuck. Here's what to actually DO:

**Minutes 0-5: Simplify the input**
- Try the smallest possible input (n=1, n=2, n=3) by hand on paper
- Does a pattern emerge? Does the answer for n=3 build on n=2?

**Minutes 5-10: Change the representation**
- Draw it. Literally draw the array, tree, or graph on paper
- If it's a string problem, write out the characters in boxes
- If it's a graph problem, draw the nodes and edges
- Visualization unlocks solutions that staring at text never will

**Minutes 10-15: Try a different pattern**
- Your first instinct might be wrong (Einstellung effect)
- Explicitly ask: "What if this is NOT a [your first guess] problem?"
- Run through the constraint decoder: what does n tell me?
- Run through signal words: what keywords am I seeing?

**Minutes 15-20: Reduce the problem**
- Can you solve a simpler version? (e.g., sorted input, no negatives, no duplicates)
- If you can solve the simpler version, what breaks when you add the constraint back?
- That "break point" is usually where the key insight lives

**After 20 minutes:** Look at the hint/solution. But then use the "Reverse Editorial" technique — trace backwards to find what you should have noticed. Write it down. Re-solve from scratch.

### When the Problem Doesn't Fit Any Known Pattern

This happens. Here's the protocol:

1. **Go back to brute force.** If you can't identify the pattern, write the O(n²) or O(2ⁿ) solution. This is always possible and shows the interviewer you can make progress.

2. **Look at the brute force and ask: "What work am I repeating?"** The repeated work tells you the optimization:
   - Repeated lookups → HashMap
   - Repeated subarray sums → Prefix Sum
   - Repeated comparisons → Sort first
   - Repeated subproblem computation → DP (memoize)

3. **Combine two patterns.** Many hard problems are two patterns stitched together:
   - Binary Search + Greedy (split array largest sum)
   - Trie + Backtracking (word search II)
   - HashMap + Sliding Window (minimum window substring)
   - Heap + Two Pointers (median from data stream)

4. **Tell the interviewer.** "I'm not immediately seeing the optimal approach, but let me start with brute force and optimize from there." This is a STRONG signal — it shows structured thinking and resilience.

### How to Do Mock Interviews

Start mock interviews after Phase 5 (you'll have enough patterns). Do at least 4 before your real interview.

**Free platforms:**
- [Pramp](https://www.pramp.com/) — free peer mock interviews, matched automatically
- [interviewing.io](https://interviewing.io/) — anonymous practice with real engineers
- Or just ask a friend to pick 2 random problems from the Top 50 table and time you

**The format:**
- 45 minutes, 2 problems (1 medium, 1 medium-hard)
- Talk out loud the entire time
- Use the 9-step process explicitly
- After each mock, write down: what went well, what didn't, what pattern you missed

### The Week Before Your Interview

```
Day 7-5: Review one phase per day (skim problems, re-solve any you forgot)
Day 4-3: Do 2 timed mock interviews (45 min each)
Day 2:   Review the Constraint Decoder table + Pattern Recognition Cheat Sheet
         Re-read your Observations Log (if you kept one)
Day 1:   Light review only. Solve 2-3 easy problems for confidence.
         Sleep well. Seriously — sleep > last-minute cramming.
```

### Track Your Progress

After each problem, mark it with one of:
- ✅ Solved without hints (you own this)
- 🟡 Solved with a hint (review in 3 days)
- ❌ Couldn't solve (review in 1 day, then again in 3 days)

**You're interview-ready when:** 80%+ of the Top 50 table is ✅, and you can identify the pattern for any new problem within 60 seconds.

---

## Common Traps That Fail Candidates (Avoid These)

| Trap | Why It Fails | Fix |
|---|---|---|
| Coding before thinking | You write 30 lines, realize the approach is wrong, panic | Spend 3-5 min explaining approach BEFORE touching code |
| Not stating complexity | Interviewer thinks you don't know it | Say "This is O(n log n) time, O(n) space" BEFORE coding |
| Ignoring edge cases | Your code crashes on empty input | Always ask: "Can input be empty? Negative? Have duplicates?" |
| Silent coding | Interviewer can't evaluate your thinking | Narrate: "Now I'm iterating through the array, checking if..." |
| Giving up when stuck | Shows no resilience | Say: "Let me think about this differently. What if I try [X]?" |
| Over-engineering | Using a segment tree when a hashmap works | Start with the simplest solution that meets the complexity requirement |
| Not testing | You submit and it fails on edge case | Trace through your code with the example input AND one edge case |

---

## Quick Tips to Fast-Track Your Journey

1. **Solve problems on paper first** for the first 2 weeks. This forces you to think before you type. It's slower but builds 10x stronger intuition.

2. **Time yourself** starting from week 3. Easy: 15 min. Medium: 25 min. Hard: 40 min. If you exceed the time, look at the solution — but re-solve from scratch after.

3. **Learn ONE language deeply** for interviews. Python is the fastest to write. Java/C++ if you're already comfortable. Don't switch languages mid-prep.

4. **Watch the NeetCode video AFTER attempting the problem**, not before. The struggle is where learning happens.

5. **Do mock interviews** starting from week 4. Use [Pramp](https://www.pramp.com/) (free) or [interviewing.io](https://interviewing.io/). Solving alone ≠ solving under pressure with someone watching.

6. **The 80/20 rule:** 6 patterns (HashMap, Two Pointers, Sliding Window, Binary Search, BFS/DFS, DP) cover 80% of all interview questions. If you're short on time, master these 6 deeply rather than all 14 shallowly.

7. **Sleep on hard problems.** If you're stuck on a DP problem at night, go to sleep. Your brain processes it overnight. You'll often see the solution clearly the next morning. This is neuroscience, not wishful thinking.

8. **After each phase, do a "blind test":** pick a random unseen problem from that topic on LeetCode. If you can identify the pattern and solve it in 25 min → you own that phase. If not → review.

---

## Your Toolkit (Bookmark These)

| Resource | What It's For | Link |
|---|---|---|
| NeetCode 150 | Curated problem list with video solutions | [neetcode.io/practice](https://neetcode.io/practice) |
| NeetCode YouTube | Best pattern-based video explanations | [youtube.com/@NeetCode](https://www.youtube.com/@NeetCode) |
| Grind 75 | Customizable study plan by time available | [techinterviewhandbook.org/grind75](https://www.techinterviewhandbook.org/grind75) |
| VisuAlgo | Visualize any algorithm step by step | [visualgo.net](https://visualgo.net/) |
| Algorithm Visualizer | Run and visualize custom code | [algorithm-visualizer.org](https://algorithm-visualizer.org/) |
| AlgoMaster Patterns | 20 patterns with templates and explanations | [blog.algomaster.io/p/20-dsa-patterns](https://blog.algomaster.io/p/20-dsa-patterns) |
| AlgoMaster DP Patterns | 20 DP-specific patterns | [blog.algomaster.io/p/20-patterns-to-master-dynamic-programming](https://blog.algomaster.io/p/20-patterns-to-master-dynamic-programming) |
| Abdul Bari YouTube | Best for conceptual understanding | [youtube.com/@abdul_bari](https://www.youtube.com/@abdul_bari) |
| takeUforward YouTube | Comprehensive DSA course (Striver) | [youtube.com/@takeUforward](https://www.youtube.com/@takeUforward) |
| Big-O Cheat Sheet | Quick complexity reference | [bigocheatsheet.com](https://www.bigocheatsheet.com/) |
| LeetCode | Practice problems | [leetcode.com](https://leetcode.com/) |

---

## How to Use This Guide

```
1. Start at Phase 1. Don't skip ahead.
2. For each pattern:
   a. Read the real-world analogy — connect it to something you know
   b. Watch the linked video (1x speed first time, 1.5x for review)
   c. Try the first problem YOURSELF for 20 minutes before watching the solution
   d. Dry run your solution on paper (use the table method from Step 2)
   e. If stuck after 20 min, watch the NeetCode solution, then re-solve from scratch
   f. Move to the next problem in the list
3. After finishing a phase, do the checkpoint exercise
4. If you fail the checkpoint, redo the phase. No shame in repetition.
5. After all phases, do 3-4 timed mock interviews (45 min, 2 problems)
```

**The rule of 3:** If you can solve 3 problems of a pattern without hints, you own that pattern. Move on.

**Don't do:** Solve problem → read solution → say "I get it" → move on. You didn't learn anything.

**Do this instead:** Solve problem → struggle → read solution → close it → solve it again from scratch → dry run it → then move on.

---

> **Remember:** You already solve problems every day at work. DSA is just the same skill
> with fancy names. A hashmap is a cache. BFS is a breadth-first deployment rollout.
> A stack is your browser history. You already know this stuff — now you're just learning
> the vocabulary to talk about it in interviews.
