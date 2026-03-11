# 🎯 The FAANG 145 — Sure Shot DSA List (With Video Solutions)

> Cross-referenced from: NeetCode 150, Blind 75, Grind 75, Striver's SDE Sheet,
> LeetCode Top Interview 150, and 2024-2025 company-specific frequency data.
>
> 🔥 = appears in 3+ curated lists (highest frequency across FAANG)
> Companies: G=Google, M=Meta, A=Amazon, Ap=Apple, Ms=Microsoft, U=Uber
> 📺 = NeetCode / best video explanation
>
> Track: ✅ solved clean | 🟡 needed hint | ❌ couldn't solve | ⬜ not attempted
>
> **Fast-track mode (20-30/day):** Read problem → watch 📺 at 1.5x → note the pattern → move on.

---

## Phase 1: Arrays & Hashing (15)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 1 | ⬜ | [Two Sum](https://leetcode.com/problems/two-sum/) | [📺](https://www.youtube.com/watch?v=KLlXCFG5TnA) | E | 🔥 | HashMap | G M A Ap Ms U |
| 2 | ⬜ | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | [📺](https://www.youtube.com/watch?v=9UtInBqnCgA) | E | 🔥 | HashMap | A M Ms |
| 3 | ⬜ | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | [📺](https://www.youtube.com/watch?v=vzdNOK2oB2E) | M | 🔥 | HashMap | G M A |
| 4 | ⬜ | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | [📺](https://www.youtube.com/watch?v=YPTqKIgVk-k) | M | 🔥 | HashMap+Heap | G M A U |
| 5 | ⬜ | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) | [📺](https://www.youtube.com/watch?v=P6RZZMu_maU) | M | 🔥 | HashSet | G M A |
| 6 | ⬜ | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) | [📺](https://www.youtube.com/watch?v=bNvIQI2wAjk) | M | 🔥 | Prefix | G M A Ap |
| 7 | ⬜ | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | [📺](https://www.youtube.com/watch?v=fFVZt-6sgyo) | M | 🔥 | Prefix+Hash | G M A U |
| 8 | ⬜ | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | [📺](https://www.youtube.com/watch?v=5WZl3MMT0Eg) | M | 🔥 | Kadane's | G A Ms Ap |
| 9 | ⬜ | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | [📺](https://www.youtube.com/watch?v=lXVy6YWFcRM) | M | | Kadane's variant | A G |
| 10 | ⬜ | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | [📺](https://www.youtube.com/watch?v=T41rL0L3Pnw) | M | | Matrix | A M Ms |
| 11 | ⬜ | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | [📺](https://www.youtube.com/watch?v=BJnMZNwUk1M) | M | | Matrix | A M Ms |
| 12 | ⬜ | [Rotate Image](https://leetcode.com/problems/rotate-image/) | [📺](https://www.youtube.com/watch?v=fMSJSS7eO1w) | M | | Matrix | A M Ms |
| 13 | ⬜ | [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/) | [📺](https://www.youtube.com/watch?v=B1k_sxOSgv8) | M | | Design | G M |
| 14 | ⬜ | [Sort Colors](https://leetcode.com/problems/sort-colors/) | [📺](https://www.youtube.com/watch?v=4xbWSRZHqac) | M | | Dutch Flag | M A Ms |
| 15 | ⬜ | [Next Permutation](https://leetcode.com/problems/next-permutation/) | [📺](https://www.youtube.com/watch?v=quAS1iydq7U) | M | | Array | G A Ms |

---

## Phase 2: Two Pointers (8)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 16 | ⬜ | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | [📺](https://www.youtube.com/watch?v=jJXJ16kPFWg) | E | 🔥 | Two Ptr | M A |
| 17 | ⬜ | [Two Sum II (Sorted)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | [📺](https://www.youtube.com/watch?v=cQ1Oz4ckceM) | M | | Two Ptr | A Ms |
| 18 | ⬜ | [3Sum](https://leetcode.com/problems/3sum/) | [📺](https://www.youtube.com/watch?v=jzZsG8n2R9A) | M | 🔥 | Sort+Two Ptr | G M A Ap U |
| 19 | ⬜ | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | [📺](https://www.youtube.com/watch?v=UuiTKBwPgAo) | M | 🔥 | Two Ptr | G M A |
| 20 | ⬜ | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | [📺](https://www.youtube.com/watch?v=ZI2z5pq0TqA) | H | 🔥 | Two Ptr/Stack | G M A Ap U |
| 21 | ⬜ | [Move Zeroes](https://leetcode.com/problems/move-zeroes/) | [📺](https://www.youtube.com/watch?v=aayNRwUN3Do) | E | | Two Ptr | M Ap |
| 22 | ⬜ | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | [📺](https://www.youtube.com/watch?v=DEJAZBq0FDA) | E | | Two Ptr | M A |
| 23 | ⬜ | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) | [📺](https://www.youtube.com/watch?v=XYQecbcd6_c) | M | 🔥 | Expand Center | G M A |

---

## Phase 3: Sliding Window (8)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 24 | ⬜ | [Longest Substring Without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | [📺](https://www.youtube.com/watch?v=wiGpQwVHdE0) | M | 🔥 | Sliding Window | G M A Ap U |
| 25 | ⬜ | [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/) | [📺](https://www.youtube.com/watch?v=gqXU1UyA8pk) | M | 🔥 | Sliding Window | G A |
| 26 | ⬜ | [Permutation in String](https://leetcode.com/problems/permutation-in-string/) | [📺](https://www.youtube.com/watch?v=UbyhOgBN834) | M | | Sliding Window | M A |
| 27 | ⬜ | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | [📺](https://www.youtube.com/watch?v=jSto0O4AJbM) | H | 🔥 | Sliding Window | G M A U S |
| 28 | ⬜ | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | [📺](https://www.youtube.com/watch?v=DfljaUwZsOk) | H | 🔥 | Monotonic Deque | G A U |
| 29 | ⬜ | [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) | [📺](https://www.youtube.com/watch?v=G8xtZy0fDKg) | M | | Sliding Window | M A |
| 30 | ⬜ | [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) | [📺](https://www.youtube.com/watch?v=aYqYMIqZx5s) | M | | Sliding Window | M A |
| 31 | ⬜ | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | [📺](https://www.youtube.com/watch?v=1pkOgXD63yU) | E | 🔥 | Sliding Window | G M A Ap Ms |

---

## Phase 4: Binary Search (10)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 32 | ⬜ | [Binary Search](https://leetcode.com/problems/binary-search/) | [📺](https://www.youtube.com/watch?v=s4DPM8ct1pI) | E | | Template | All |
| 33 | ⬜ | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | [📺](https://www.youtube.com/watch?v=U8XENwh8Oy8) | M | 🔥 | Modified BS | G M A U |
| 34 | ⬜ | [Find Min in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | [📺](https://www.youtube.com/watch?v=nIVW4P8b1VA) | M | 🔥 | Modified BS | G M A |
| 35 | ⬜ | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | [📺](https://www.youtube.com/watch?v=Ber2pi2C0j0) | M | | BS | A Ms |
| 36 | ⬜ | [Find Peak Element](https://leetcode.com/problems/find-peak-element/) | [📺](https://www.youtube.com/watch?v=HtSuA80QTyo) | M | | BS | G M |
| 37 | ⬜ | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | [📺](https://www.youtube.com/watch?v=U2SozAs9RzA) | M | 🔥 | BS on Answer | G A U |
| 38 | ⬜ | [Capacity To Ship Packages](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | [📺](https://www.youtube.com/watch?v=ER_oLmdc-nw) | M | | BS on Answer | A U |
| 39 | ⬜ | [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) | [📺](https://www.youtube.com/watch?v=fu2cD_6E8Hw) | M | | BS | G A |
| 40 | ⬜ | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | [📺](https://www.youtube.com/watch?v=q6IEA26hvXc) | H | 🔥 | BS | G M A Ap |
| 41 | ⬜ | [Single Element in Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/) | [📺](https://www.youtube.com/watch?v=HGtqdzyUJ3k) | M | | BS | A Ms |

---

## Phase 5: Stack (9)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 42 | ⬜ | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | [📺](https://www.youtube.com/watch?v=WTzjTskDFMg) | E | 🔥 | Stack | G M A Ap Ms |
| 43 | ⬜ | [Min Stack](https://leetcode.com/problems/min-stack/) | [📺](https://www.youtube.com/watch?v=qkLl7nAwDPo) | M | 🔥 | Stack Design | A M Ms |
| 44 | ⬜ | [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) | [📺](https://www.youtube.com/watch?v=iu0082c4HDE) | M | | Stack | A Ms |
| 45 | ⬜ | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | [📺](https://www.youtube.com/watch?v=cTBiBSnjO3c) | M | 🔥 | Monotonic Stack | G A |
| 46 | ⬜ | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | [📺](https://www.youtube.com/watch?v=zx5Sw9130L0) | H | 🔥 | Monotonic Stack | G A M |
| 47 | ⬜ | [Decode String](https://leetcode.com/problems/decode-string/) | [📺](https://www.youtube.com/watch?v=qB0zZpBJlh8) | M | | Stack | G A |
| 48 | ⬜ | [Remove K Digits](https://leetcode.com/problems/remove-k-digits/) | [📺](https://www.youtube.com/watch?v=cFabMOnJaq0) | M | | Greedy Stack | G A |
| 49 | ⬜ | [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | [📺](https://www.youtube.com/watch?v=rbEJH4AF5KQ) | M | | Stack | M A |
| 50 | ⬜ | [Simplify Path](https://leetcode.com/problems/simplify-path/) | [📺](https://www.youtube.com/watch?v=qYlHrAKJfyA) | M | | Stack | M A |

---

## Phase 6: Linked List (10)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 51 | ⬜ | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | [📺](https://www.youtube.com/watch?v=G0_I-ZF0S38) | E | 🔥 | Reversal | G M A Ap Ms |
| 52 | ⬜ | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | [📺](https://www.youtube.com/watch?v=XIdigk956u0) | E | 🔥 | Merge | A M Ms |
| 53 | ⬜ | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | [📺](https://www.youtube.com/watch?v=gBTe7lFR3vc) | E | 🔥 | Fast/Slow | M A Ms |
| 54 | ⬜ | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | [📺](https://www.youtube.com/watch?v=wjYnzkAhcNk) | M | | Fast/Slow | M A |
| 55 | ⬜ | [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | [📺](https://www.youtube.com/watch?v=XVuQxVej6y8) | M | | Two Ptr | M A |
| 56 | ⬜ | [Reorder List](https://leetcode.com/problems/reorder-list/) | [📺](https://www.youtube.com/watch?v=S5bfdUTrKLM) | M | | Mid+Rev+Merge | M A |
| 57 | ⬜ | [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) | [📺](https://www.youtube.com/watch?v=wgFPrzTjm7s) | M | 🔥 | Traversal | M A Ap |
| 58 | ⬜ | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | [📺](https://www.youtube.com/watch?v=5Y2EiZST97Y) | M | | HashMap | M A |
| 59 | ⬜ | [LRU Cache](https://leetcode.com/problems/lru-cache/) | [📺](https://www.youtube.com/watch?v=7ABFKPK2hD4) | M | 🔥 | DLL+HashMap | G M A Ap Ms U |
| 60 | ⬜ | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | [📺](https://www.youtube.com/watch?v=q5a5OiGbT6Q) | H | 🔥 | Heap+LL | G M A |

---

## Phase 7: Trees (18)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 61 | ⬜ | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | [📺](https://www.youtube.com/watch?v=hTM3phVI6YQ) | E | 🔥 | DFS | All |
| 62 | ⬜ | [Same Tree](https://leetcode.com/problems/same-tree/) | [📺](https://www.youtube.com/watch?v=vRbbcKXCxOw) | E | | DFS | M A |
| 63 | ⬜ | [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) | [📺](https://www.youtube.com/watch?v=OnSn2XEQ4MY) | E | 🔥 | DFS | G A |
| 64 | ⬜ | [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/) | [📺](https://www.youtube.com/watch?v=Mao9myH_VKo) | E | | DFS | A M Ms |
| 65 | ⬜ | [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) | [📺](https://www.youtube.com/watch?v=bkxqA8Rfv04) | E | 🔥 | DFS | M G A |
| 66 | ⬜ | [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) | [📺](https://www.youtube.com/watch?v=QfJsau0ItOY) | E | | DFS | A M |
| 67 | ⬜ | [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/) | [📺](https://www.youtube.com/watch?v=E36O5SWp-LE) | E | | DFS | M A |
| 68 | ⬜ | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | [📺](https://www.youtube.com/watch?v=6ZnyEApgFYg) | M | 🔥 | BFS | G M A Ms |
| 69 | ⬜ | [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/) | [📺](https://www.youtube.com/watch?v=d4zLyf32e3I) | M | 🔥 | BFS | M A |
| 70 | ⬜ | [Binary Tree Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) | [📺](https://www.youtube.com/watch?v=igbboQbEqeI) | M | | BFS | M A Ms |
| 71 | ⬜ | [Path Sum](https://leetcode.com/problems/path-sum/) | [📺](https://www.youtube.com/watch?v=LSKQyOz_P8I) | E | | DFS | A M |
| 72 | ⬜ | [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) | [📺](https://www.youtube.com/watch?v=gs2LMfuOR9k) | M | 🔥 | DFS | G M A Ap Ms |
| 73 | ⬜ | [Binary Tree Max Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | [📺](https://www.youtube.com/watch?v=Hr5cWUld4vU) | H | 🔥 | DFS | G M A |
| 74 | ⬜ | [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) | [📺](https://www.youtube.com/watch?v=s6ATEkipzow) | M | 🔥 | DFS+Range | G M A Ms |
| 75 | ⬜ | [Kth Smallest in BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) | [📺](https://www.youtube.com/watch?v=5LUXSvjmGCw) | M | 🔥 | Inorder | G M A |
| 76 | ⬜ | [Construct from Preorder & Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | [📺](https://www.youtube.com/watch?v=ihj4IQGZ2zc) | M | 🔥 | Recursion | G M A |
| 77 | ⬜ | [Serialize & Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | [📺](https://www.youtube.com/watch?v=u4JAi2JJhI8) | H | 🔥 | BFS/DFS | G M A |
| 78 | ⬜ | [Count Good Nodes](https://leetcode.com/problems/count-good-nodes-in-binary-tree/) | [📺](https://www.youtube.com/watch?v=7cp5imvDzl4) | M | | DFS | G A |

---

## Phase 8: Heaps (7)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 79 | ⬜ | [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/) | [📺](https://www.youtube.com/watch?v=XEmy13g1Qc8) | M | 🔥 | Heap/Quickselect | G M A |
| 80 | ⬜ | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | [📺](https://www.youtube.com/watch?v=rI2EBUEMfTk) | M | 🔥 | Heap | M A U |
| 81 | ⬜ | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | [📺](https://www.youtube.com/watch?v=itmhHWaHupI) | H | 🔥 | Two Heaps | G M A |
| 82 | ⬜ | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | [📺](https://www.youtube.com/watch?v=s8p8ukTyA2I) | M | 🔥 | Heap+Greedy | M A |
| 83 | ⬜ | [Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/) | [📺](https://www.youtube.com/watch?v=cupg2TGIkyg) | M | | Heap | A G |
| 84 | ⬜ | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | [📺](https://www.youtube.com/watch?v=FdzJmTCVyJU) | M | 🔥 | Heap/Sweep | G M A U |
| 85 | ⬜ | [IPO](https://leetcode.com/problems/ipo/) | [📺](https://www.youtube.com/watch?v=1IUzNJ6TPEM) | H | | Two Heaps | A G |

---

## Phase 9: Backtracking (8)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 86 | ⬜ | [Subsets](https://leetcode.com/problems/subsets/) | [📺](https://www.youtube.com/watch?v=REOH22Xwdkk) | M | 🔥 | Backtrack | G M A |
| 87 | ⬜ | [Subsets II](https://leetcode.com/problems/subsets-ii/) | [📺](https://www.youtube.com/watch?v=Vn2v6ajA7U0) | M | | Backtrack+Dedup | A M |
| 88 | ⬜ | [Combination Sum](https://leetcode.com/problems/combination-sum/) | [📺](https://www.youtube.com/watch?v=GBKI9VSKdGg) | M | 🔥 | Backtrack | G M A |
| 89 | ⬜ | [Permutations](https://leetcode.com/problems/permutations/) | [📺](https://www.youtube.com/watch?v=s7AvT7cGdSo) | M | 🔥 | Backtrack | G M A |
| 90 | ⬜ | [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) | [📺](https://www.youtube.com/watch?v=s9fokUqJ76A) | M | 🔥 | Backtrack | G M A U |
| 91 | ⬜ | [Letter Combinations of Phone](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | [📺](https://www.youtube.com/watch?v=0snEunUacZY) | M | 🔥 | Backtrack | G M A |
| 92 | ⬜ | [Word Search](https://leetcode.com/problems/word-search/) | [📺](https://www.youtube.com/watch?v=pfiQ_PS1g8E) | M | 🔥 | Grid Backtrack | G M A |
| 93 | ⬜ | [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/) | [📺](https://www.youtube.com/watch?v=3jvWodd7ht0) | M | | Backtrack | G A |

---

## Phase 10: Graphs (16)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 94 | ⬜ | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | [📺](https://www.youtube.com/watch?v=pV2kpPD66nE) | M | 🔥 | BFS/DFS | G M A Ap Ms U |
| 95 | ⬜ | [Max Area of Island](https://leetcode.com/problems/max-area-of-island/) | [📺](https://www.youtube.com/watch?v=iJGr1OtmH0c) | M | | DFS | A M |
| 96 | ⬜ | [Flood Fill](https://leetcode.com/problems/flood-fill/) | [📺](https://www.youtube.com/watch?v=aehEcTEPtCs) | E | | DFS | G A |
| 97 | ⬜ | [Clone Graph](https://leetcode.com/problems/clone-graph/) | [📺](https://www.youtube.com/watch?v=mQeF6bN8hMk) | M | 🔥 | DFS+HashMap | G M A |
| 98 | ⬜ | [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) | [📺](https://www.youtube.com/watch?v=s-VkcjHqkGI) | M | 🔥 | Multi-DFS | G A |
| 99 | ⬜ | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) | [📺](https://www.youtube.com/watch?v=9z2BunfoZ5Y) | M | | DFS | G A |
| 100 | ⬜ | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | [📺](https://www.youtube.com/watch?v=y704fEOx0s0) | M | 🔥 | Multi-BFS | G M A U |
| 101 | ⬜ | [Course Schedule](https://leetcode.com/problems/course-schedule/) | [📺](https://www.youtube.com/watch?v=EgI5nU9etnU) | M | 🔥 | Topo Sort | G M A U |
| 102 | ⬜ | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | [📺](https://www.youtube.com/watch?v=Akt3glAwyfY) | M | 🔥 | Topo Sort | G M A |
| 103 | ⬜ | [Word Ladder](https://leetcode.com/problems/word-ladder/) | [📺](https://www.youtube.com/watch?v=h9iTnkgv05E) | H | 🔥 | BFS | G M A U |
| 104 | ⬜ | [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) | [📺](https://www.youtube.com/watch?v=8f1XPm4WOUc) | M | | Union-Find/DFS | G M A |
| 105 | ⬜ | [Redundant Connection](https://leetcode.com/problems/redundant-connection/) | [📺](https://www.youtube.com/watch?v=FXWRE67PLL0) | M | | Union-Find | G A |
| 106 | ⬜ | [Accounts Merge](https://leetcode.com/problems/accounts-merge/) | [📺](https://www.youtube.com/watch?v=6st4IxEF-90) | M | | Union-Find | M A |
| 107 | ⬜ | [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/) | [📺](https://www.youtube.com/watch?v=mev55LTubBY) | M | | BFS/DFS | G M |
| 108 | ⬜ | [Network Delay Time](https://leetcode.com/problems/network-delay-time/) | [📺](https://www.youtube.com/watch?v=EaphyqKU4PQ) | M | 🔥 | Dijkstra | G A U |
| 109 | ⬜ | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) | [📺](https://www.youtube.com/watch?v=5eIK3zUdYmE) | M | | BFS/Bellman | G A U |

---

## Phase 11: Dynamic Programming (20)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| | | **1D DP** | | | | | |
| 110 | ⬜ | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | [📺](https://www.youtube.com/watch?v=Y0lT9Fck7qI) | E | 🔥 | 1D DP | G A Ms |
| 111 | ⬜ | [House Robber](https://leetcode.com/problems/house-robber/) | [📺](https://www.youtube.com/watch?v=73r3KWiEvyk) | M | 🔥 | 1D DP | G M A |
| 112 | ⬜ | [House Robber II](https://leetcode.com/problems/house-robber-ii/) | [📺](https://www.youtube.com/watch?v=rWAJCfYYOvM) | M | | 1D DP | A M |
| 113 | ⬜ | [Decode Ways](https://leetcode.com/problems/decode-ways/) | [📺](https://www.youtube.com/watch?v=6aEyTjOwlJU) | M | 🔥 | 1D DP | G M A |
| 114 | ⬜ | [Coin Change](https://leetcode.com/problems/coin-change/) | [📺](https://www.youtube.com/watch?v=H9bfqozjoqs) | M | 🔥 | 1D DP | G M A Ap U |
| 115 | ⬜ | [Coin Change II](https://leetcode.com/problems/coin-change-ii/) | [📺](https://www.youtube.com/watch?v=Mjy4hd2xgrs) | M | | 1D DP | A G |
| 116 | ⬜ | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | [📺](https://www.youtube.com/watch?v=cjWnW0hdF1Y) | M | 🔥 | 1D DP | G M A |
| 117 | ⬜ | [Word Break](https://leetcode.com/problems/word-break/) | [📺](https://www.youtube.com/watch?v=Sx9NNgInc3A) | M | 🔥 | 1D DP | G M A Ap U |
| 118 | ⬜ | [Jump Game](https://leetcode.com/problems/jump-game/) | [📺](https://www.youtube.com/watch?v=Yan0cv2cLy8) | M | 🔥 | Greedy/DP | G A M |
| 119 | ⬜ | [Jump Game II](https://leetcode.com/problems/jump-game-ii/) | [📺](https://www.youtube.com/watch?v=dJ7sWiOoK7g) | M | | Greedy | G A |
| | | **2D / Grid DP** | | | | | |
| 120 | ⬜ | [Unique Paths](https://leetcode.com/problems/unique-paths/) | [📺](https://www.youtube.com/watch?v=IlEsdxuD4lY) | M | 🔥 | Grid DP | G M A |
| 121 | ⬜ | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | [📺](https://www.youtube.com/watch?v=pGMsrvt0fpk) | M | | Grid DP | G A |
| 122 | ⬜ | [Maximal Square](https://leetcode.com/problems/maximal-square/) | [📺](https://www.youtube.com/watch?v=6X7Ha2PrDmM) | M | | Grid DP | G A |
| | | **String DP** | | | | | |
| 123 | ⬜ | [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | [📺](https://www.youtube.com/watch?v=Ua0GhsJSlWM) | M | 🔥 | String DP | G M A |
| 124 | ⬜ | [Edit Distance](https://leetcode.com/problems/edit-distance/) | [📺](https://www.youtube.com/watch?v=XYi2-LPrwm4) | M | 🔥 | String DP | G M A |
| 125 | ⬜ | [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | [📺](https://www.youtube.com/watch?v=bUr8cNWI09Q) | M | | String DP | A G |
| | | **Knapsack** | | | | | |
| 126 | ⬜ | [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) | [📺](https://www.youtube.com/watch?v=IsvocB5BJhw) | M | 🔥 | 0/1 Knapsack | G A |
| 127 | ⬜ | [Target Sum](https://leetcode.com/problems/target-sum/) | [📺](https://www.youtube.com/watch?v=g0npyaQtAQM) | M | | Knapsack | G M A |
| | | **Stock DP** | | | | | |
| 128 | ⬜ | [Buy/Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | [📺](https://www.youtube.com/watch?v=I7j0F7AHpb8) | M | | State Machine | A G |
| | | **Interval DP** | | | | | |
| 129 | ⬜ | [Burst Balloons](https://leetcode.com/problems/burst-balloons/) | [📺](https://www.youtube.com/watch?v=VFskby7lUbw) | H | | Interval DP | G A |

---

## Phase 12: Greedy & Intervals (8)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 130 | ⬜ | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | [📺](https://www.youtube.com/watch?v=44H3cEC2fFM) | M | 🔥 | Sort+Scan | G M A Ap Ms U |
| 131 | ⬜ | [Insert Interval](https://leetcode.com/problems/insert-interval/) | [📺](https://www.youtube.com/watch?v=A8NUOmlR6KA) | M | 🔥 | Intervals | G A |
| 132 | ⬜ | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | [📺](https://www.youtube.com/watch?v=nONCGxWoUfM) | M | | Greedy | G A |
| 133 | ⬜ | [Gas Station](https://leetcode.com/problems/gas-station/) | [📺](https://www.youtube.com/watch?v=lJwbPZGo05A) | M | | Greedy | A G |
| 134 | ⬜ | [Candy](https://leetcode.com/problems/candy/) | [📺](https://www.youtube.com/watch?v=QzPWc0ilEek) | H | | Greedy | A G |
| 135 | ⬜ | [Hand of Straights](https://leetcode.com/problems/hand-of-straights/) | [📺](https://www.youtube.com/watch?v=amnrMCVd2YI) | M | | Greedy+Map | G A |
| 136 | ⬜ | [Partition Labels](https://leetcode.com/problems/partition-labels/) | [📺](https://www.youtube.com/watch?v=B7m8UmZE-vw) | M | | Greedy | A G |
| 137 | ⬜ | [Min Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | [📺](https://www.youtube.com/watch?v=lPmkKnvNPrw) | M | | Greedy | A G |

---

## Phase 13: Trie & Bit Manipulation (6)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 138 | ⬜ | [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/) | [📺](https://www.youtube.com/watch?v=oobqoCJlHA0) | M | 🔥 | Trie | G M A |
| 139 | ⬜ | [Design Add and Search Words](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | [📺](https://www.youtube.com/watch?v=BTf05gs_8iU) | M | | Trie+DFS | G M |
| 140 | ⬜ | [Word Search II](https://leetcode.com/problems/word-search-ii/) | [📺](https://www.youtube.com/watch?v=asbcE9mZz_U) | H | 🔥 | Trie+Backtrack | G M A |
| 141 | ⬜ | [Single Number](https://leetcode.com/problems/single-number/) | [📺](https://www.youtube.com/watch?v=qMPX1AOa83k) | E | | XOR | A M |
| 142 | ⬜ | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | [📺](https://www.youtube.com/watch?v=5Km3utixwZs) | E | | Bit Manip | A |
| 143 | ⬜ | [Missing Number](https://leetcode.com/problems/missing-number/) | [📺](https://www.youtube.com/watch?v=WnPLSRLSANE) | E | | XOR/Math | A M |

---

## Bonus: Design (2)

| # | Status | Problem | 📺 Video | Diff | Freq | Pattern | Co. |
|---|--------|---------|----------|------|------|---------|-----|
| 144 | ⬜ | [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) | [📺](https://www.youtube.com/watch?v=j4KwhBziOpg) | M | 🔥 | HashMap+Array | M A G |
| 145 | ⬜ | [Design Twitter](https://leetcode.com/problems/design-twitter/) | [📺](https://www.youtube.com/watch?v=pNichitDD2E) | M | | Heap+Design | A |

---

## Stats

| Category | Count |
|----------|-------|
| Arrays & Hashing | 15 |
| Two Pointers | 8 |
| Sliding Window | 8 |
| Binary Search | 10 |
| Stack | 9 |
| Linked List | 10 |
| Trees | 18 |
| Heaps | 7 |
| Backtracking | 8 |
| Graphs | 16 |
| Dynamic Programming | 20 |
| Greedy & Intervals | 8 |
| Trie & Bits | 6 |
| Design | 2 |
| **TOTAL** | **145** |

---

## ⚡ Fast-Track Schedule (20-30 problems/day)

```
Day 1:  Phase 1 (15) — Arrays & Hashing
Day 2:  Phase 2+3 (16) — Two Pointers + Sliding Window
Day 3:  Phase 4+5 (19) — Binary Search + Stack
Day 4:  Phase 6+7 (28) — Linked List + Trees (split across day if needed)
Day 5:  Phase 8+9 (15) — Heaps + Backtracking
Day 6:  Phase 10 (16) — Graphs
Day 7:  Phase 11 (20) — Dynamic Programming
Day 8:  Phase 12+13+Bonus (16) — Greedy + Trie + Bits + Design
```

**Method per problem (~7-10 min each):**
1. Read problem (1 min)
2. Look at constraints → guess pattern (30 sec)
3. Watch 📺 at 1.5x speed (5-7 min)
4. Write one line: "Signal → Pattern → Why"
5. Move on. Don't code. Just absorb.

**Second pass (week 2):** Go back to 🔥 problems only. This time, code them.

---

> Sources: NeetCode 150, Blind 75, Grind 75, Striver's SDE Sheet,
> LeetCode Top Interview 150, company-tagged frequency data (2024-2025)
