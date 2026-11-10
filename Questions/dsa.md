# Data Structures & Algorithms — Question Bank (Fundamentals → Senior → Staff Backend)

Each category starts with concept questions and then lists problems in rough order of difficulty. Part A covers fundamentals. Part B covers core interview topics. Part C covers advanced topics. Part D covers senior+ and staff-level questions that connect DSA to real systems.

---

# Part A — Fundamentals

## 1. Complexity Analysis

1. What is Big-O notation, and how do Big-O, Big-Ω, and Big-Θ differ?
2. What are the common complexity classes (O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ), O(n!)), with an example of each?
3. What is the difference between time complexity and space complexity?
4. What is amortized analysis, and why is appending to a dynamic array O(1) amortized?
5. What is the difference between best-case, average-case, and worst-case complexity?
6. How do you analyze the complexity of recursive functions (recurrence relations, the Master Theorem)?
7. Why does recursion use O(depth) extra space?
8. Why can an O(n log n) algorithm beat an O(n) algorithm in practice (constants, cache behavior)?
9. Given input size limits (n ≤ 20, 10³, 10⁵, 10⁹), what complexity should your solution target?

## 2. Arrays & Strings

10. How are arrays laid out in memory, and why is index access O(1)?
11. How does a dynamic array grow, and what growth factor is typical?
12. Why is inserting at the front of an array O(n)?
13. How are strings stored, and why is repeated string concatenation O(n²) in many languages?
14. Problem: Two Sum (return indices of two numbers adding to a target).
15. Problem: Best Time to Buy and Sell Stock.
16. Problem: Contains Duplicate.
17. Problem: Product of Array Except Self without using division.
18. Problem: Maximum Subarray (Kadane's algorithm).
19. Problem: Rotate an array by k steps in place.
20. Problem: Merge Intervals.
21. Problem: Insert Interval.
22. Problem: Valid Anagram and Group Anagrams.
23. Problem: Longest Common Prefix.
24. Problem: Valid Palindrome, and Valid Palindrome II (one deletion allowed).
25. Problem: String to Integer (atoi) — which edge cases matter?
26. Problem: Spiral Matrix and Rotate Image (rotate a matrix 90° in place).
27. Problem: Set Matrix Zeroes with O(1) extra space.
28. Problem: First Missing Positive in O(n) time and O(1) space.

## 3. Hashing

29. How does a hash table work, and what makes a good hash function?
30. What are separate chaining and open addressing (linear probing, quadratic probing, Robin Hood hashing)?
31. What is the load factor, and when does a hash table resize?
32. Why are hash table operations O(1) on average but O(n) in the worst case?
33. What is a hash-flooding (collision) attack, and how do randomized hash seeds prevent it?
34. Hash set vs sorted set (balanced tree): when do you choose each?
35. Problem: Longest Consecutive Sequence in O(n).
36. Problem: Subarray Sum Equals K (prefix sums + hash map).
37. Problem: Top K Frequent Elements.
38. Problem: Isomorphic Strings and Word Pattern.
39. Problem: Design a hash map without using built-in hash tables.

## 4. Two Pointers & Sliding Window

40. When does the two-pointer technique apply, and why does it often need sorted input?
41. What is the difference between a fixed-size and a variable-size sliding window?
42. Problem: Container With Most Water.
43. Problem: 3Sum, and how do you avoid duplicate triplets?
44. Problem: Remove Duplicates from Sorted Array in place.
45. Problem: Trapping Rain Water (two pointers and stack approaches).
46. Problem: Longest Substring Without Repeating Characters.
47. Problem: Longest Repeating Character Replacement.
48. Problem: Minimum Window Substring.
49. Problem: Permutation in String (find an anagram of one string in another).
50. Problem: Sliding Window Maximum (monotonic deque).
51. Problem: Subarrays with at most K distinct integers, and "exactly K = atMost(K) − atMost(K−1)".

## 5. Linked Lists

52. What are singly, doubly, and circular linked lists, and what are their trade-offs vs arrays?
53. Why are linked lists slow in practice despite O(1) insertion (cache misses)?
54. What is a dummy (sentinel) node, and why does it simplify list code?
55. Problem: Reverse a Linked List (iterative and recursive).
56. Problem: Merge Two Sorted Lists.
57. Problem: Linked List Cycle, and find the start of the cycle (Floyd's algorithm). Why does it work?
58. Problem: Remove the Nth Node From the End of a List in one pass.
59. Problem: Find the Middle of a Linked List.
60. Problem: Reorder List (L0 → Ln → L1 → Ln−1 …).
61. Problem: Intersection of Two Linked Lists.
62. Problem: Add Two Numbers represented as linked lists.
63. Problem: Copy a List with Random Pointer.
64. Problem: Reverse Nodes in k-Group.
65. Problem: Merge k Sorted Lists.

## 6. Stacks & Queues

66. What are stacks and queues, and how do you implement each with arrays or linked lists?
67. How do you implement a queue using two stacks, and what is the amortized cost?
68. What is a deque, and what is a circular buffer?
69. What is a monotonic stack, and which kinds of problems does it solve?
70. Problem: Valid Parentheses.
71. Problem: Min Stack (get the minimum in O(1)).
72. Problem: Evaluate Reverse Polish Notation.
73. Problem: Daily Temperatures (next greater element).
74. Problem: Largest Rectangle in Histogram.
75. Problem: Basic Calculator (with parentheses and operators).
76. Problem: Decode String (`3[a2[c]]`).
77. Problem: Asteroid Collision.

## 7. Recursion & Backtracking

78. What are the base case and recursive case, and how does the call stack work?
79. What is the difference between recursion and iteration, and how do you convert one to the other?
80. What is tail recursion, and which languages optimize it?
81. What is backtracking, and how does pruning reduce the search space?
82. Problem: Generate all Subsets (power set), with and without duplicates.
83. Problem: Generate all Permutations, with and without duplicates.
84. Problem: Combination Sum (I and II).
85. Problem: Generate Parentheses.
86. Problem: Letter Combinations of a Phone Number.
87. Problem: Word Search in a grid.
88. Problem: Palindrome Partitioning.
89. Problem: N-Queens.
90. Problem: Sudoku Solver.

---

# Part B — Core Interview Topics

## 8. Sorting & Searching

91. How do bubble, insertion, and selection sort work, and when is insertion sort actually useful?
92. How does merge sort work, and why is it stable and O(n log n) in the worst case?
93. How does quicksort work, why is its worst case O(n²), and how do randomized pivots avoid it?
94. What is the difference between Lomuto and Hoare partitioning?
95. How does heap sort work, and why is it rarely used in practice despite O(n log n)?
96. What does it mean for a sort to be stable, and when does stability matter?
97. Why is comparison-based sorting bounded by Ω(n log n)?
98. How do counting sort, radix sort, and bucket sort beat O(n log n)?
99. What sorting algorithms do real languages use (Timsort, introsort, pdqsort), and why?
100. How does external sorting work when data doesn't fit in memory?
101. How does binary search work, and what are the common off-by-one pitfalls?
102. What is "binary search on the answer," and how do you recognize when it applies?
103. Problem: Search in a Rotated Sorted Array.
104. Problem: Find Minimum in a Rotated Sorted Array.
105. Problem: Find First and Last Position of an Element in a Sorted Array.
106. Problem: Koko Eating Bananas (binary search on the answer).
107. Problem: Median of Two Sorted Arrays in O(log(min(m, n))).
108. Problem: Kth Largest Element in an Array (Quickselect vs heap).
109. Problem: Sort Colors (Dutch national flag).
110. Problem: Count inversions in an array (merge sort).

## 9. Trees

111. What are binary trees, full, complete, perfect, and balanced trees?
112. What are pre-order, in-order, post-order, and level-order traversals, and when do you use each?
113. How do you traverse a tree iteratively, and what is Morris traversal?
114. What is the height vs depth of a node?
115. Problem: Maximum Depth of a Binary Tree.
116. Problem: Invert a Binary Tree.
117. Problem: Same Tree and Subtree of Another Tree.
118. Problem: Diameter of a Binary Tree.
119. Problem: Balanced Binary Tree check.
120. Problem: Binary Tree Level Order Traversal and Zigzag Traversal.
121. Problem: Binary Tree Right Side View.
122. Problem: Lowest Common Ancestor of a Binary Tree.
123. Problem: Path Sum (I, II, III).
124. Problem: Binary Tree Maximum Path Sum.
125. Problem: Construct a Binary Tree from Preorder and Inorder Traversal.
126. Problem: Serialize and Deserialize a Binary Tree.
127. Problem: Count Good Nodes in a Binary Tree.

## 10. Binary Search Trees & Balanced Trees

128. What is the BST property, and what are search, insert, and delete complexities?
129. Why does an unbalanced BST degrade to O(n)?
130. How do AVL trees and red-black trees stay balanced, and how do they compare?
131. What are B-trees and B+ trees, and why do databases and file systems use them?
132. What is a skip list, and why do Redis and some databases use it instead of balanced trees?
133. Problem: Validate a Binary Search Tree.
134. Problem: Kth Smallest Element in a BST.
135. Problem: Lowest Common Ancestor of a BST.
136. Problem: Convert a Sorted Array to a Balanced BST.
137. Problem: BST Iterator with O(h) memory.
138. Problem: Delete a Node in a BST.

## 11. Heaps & Priority Queues

139. What is a binary heap, and how is it stored in an array?
140. What are the complexities of insert, extract-min, peek, and heapify?
141. Why is building a heap from an array O(n) and not O(n log n)?
142. When do you use a min-heap vs a max-heap, and how do you get the top K elements efficiently?
143. Problem: Kth Largest Element in a Stream.
144. Problem: Last Stone Weight.
145. Problem: K Closest Points to Origin.
146. Problem: Task Scheduler.
147. Problem: Find Median from a Data Stream (two heaps).
148. Problem: Merge k sorted arrays or lists.
149. Problem: Reorganize String so no two adjacent characters are the same.
150. Problem: IPO / Maximize Capital (two heaps greedy).

## 12. Graphs

151. How do you represent graphs (adjacency list, adjacency matrix, edge list), and when do you use each?
152. What is the difference between directed, undirected, weighted, cyclic, and acyclic graphs?
153. How do BFS and DFS work, and what are their complexities?
154. Why does BFS find shortest paths in unweighted graphs?
155. How do you detect a cycle in a directed graph vs an undirected graph?
156. What is topological sorting, and how do Kahn's algorithm and DFS-based ordering work?
157. How does Dijkstra's algorithm work, and why does it fail with negative edge weights?
158. How do Bellman-Ford and Floyd-Warshall work, and when would you use them?
159. What is A* search, and how does a heuristic guide it?
160. What are minimum spanning trees, and how do Kruskal's and Prim's algorithms work?
161. What is Union-Find (disjoint set union), and how do path compression and union by rank make it nearly O(1)?
162. What are strongly connected components (Tarjan's, Kosaraju's algorithms)?
163. What are bridges and articulation points, and why do they matter for network reliability?
164. What is a bipartite graph, and how do you check for one?
165. Problem: Number of Islands.
166. Problem: Clone Graph.
167. Problem: Max Area of Island.
168. Problem: Pacific Atlantic Water Flow.
169. Problem: Rotting Oranges (multi-source BFS).
170. Problem: Course Schedule (I and II).
171. Problem: Number of Connected Components in an Undirected Graph.
172. Problem: Graph Valid Tree.
173. Problem: Redundant Connection (Union-Find).
174. Problem: Word Ladder.
175. Problem: Network Delay Time (Dijkstra).
176. Problem: Cheapest Flights Within K Stops.
177. Problem: Min Cost to Connect All Points (MST).
178. Problem: Alien Dictionary (topological sort).
179. Problem: Swim in Rising Water.
180. Problem: Reconstruct Itinerary (Eulerian path).

## 13. Dynamic Programming

181. What two properties make a problem suitable for dynamic programming (optimal substructure, overlapping subproblems)?
182. What is the difference between memoization (top-down) and tabulation (bottom-up)?
183. How do you define a DP state and its transitions?
184. How do you reduce DP space complexity (rolling arrays)?
185. How do you tell DP problems apart from greedy problems?
186. Problem: Climbing Stairs and Min Cost Climbing Stairs.
187. Problem: House Robber (I and II).
188. Problem: Coin Change (minimum coins) and Coin Change II (number of ways).
189. Problem: Longest Increasing Subsequence in O(n log n).
190. Problem: Longest Common Subsequence.
191. Problem: Edit Distance.
192. Problem: Word Break.
193. Problem: Decode Ways.
194. Problem: Unique Paths and Unique Paths with Obstacles.
195. Problem: Partition Equal Subset Sum (0/1 knapsack).
196. Problem: Target Sum.
197. Problem: Longest Palindromic Substring and Palindromic Substrings count.
198. Problem: Maximum Product Subarray.
199. Problem: Best Time to Buy and Sell Stock with cooldown or transaction limits (state machine DP).
200. Problem: Interleaving String.
201. Problem: Distinct Subsequences.
202. Problem: Burst Balloons (interval DP).
203. Problem: Regular Expression Matching.
204. Problem: Longest Increasing Path in a Matrix.

## 14. Greedy Algorithms

205. What is a greedy algorithm, and how do you prove a greedy choice is correct (exchange argument)?
206. Why does greedy fail for coin change with arbitrary denominations?
207. Problem: Jump Game (I and II).
208. Problem: Gas Station.
209. Problem: Non-overlapping Intervals (interval scheduling).
210. Problem: Meeting Rooms II (minimum rooms needed).
211. Problem: Partition Labels.
212. Problem: Hand of Straights.
213. Problem: Candy distribution.

## 15. Tries & String Algorithms

214. What is a trie, and what are its time and space trade-offs?
215. How do compressed tries (radix trees) save space, and where are they used (routers, HTTP routing)?
216. How does the KMP algorithm avoid re-scanning text?
217. How does Rabin-Karp use rolling hashes, and what are its collision risks?
218. What are the Z-algorithm and Manacher's algorithm used for?
219. What are suffix arrays and suffix trees, and what problems do they solve?
220. Problem: Implement a Trie (insert, search, startsWith).
221. Problem: Design Add and Search Words with wildcard support.
222. Problem: Word Search II (trie + backtracking).
223. Problem: Autocomplete system returning top suggestions by frequency.
224. Problem: Find the index of the first occurrence of a pattern in a string (KMP).

## 16. Bit Manipulation & Math

225. What are AND, OR, XOR, NOT, and shifts, and what are common bit tricks?
226. How do you check whether a number is a power of two?
227. How are negative numbers represented (two's complement)?
228. Problem: Single Number (XOR).
229. Problem: Number of 1 Bits and Counting Bits.
230. Problem: Reverse Bits.
231. Problem: Missing Number.
232. Problem: Sum of Two Integers without `+`.
233. Problem: Pow(x, n) with fast exponentiation.
234. How do you compute GCD (Euclid's algorithm) and generate primes (Sieve of Eratosthenes)?
235. What is modular arithmetic, and why do problems ask for answers modulo 10⁹ + 7?
236. How does reservoir sampling pick k random items from a stream of unknown length?
237. How does the Fisher–Yates shuffle produce an unbiased permutation?

---

# Part C — Advanced Topics

## 17. Advanced Data Structures

238. What is a segment tree, and how does it answer range queries with updates in O(log n)?
239. What is lazy propagation in segment trees?
240. What is a Fenwick tree (binary indexed tree), and how does it compare with a segment tree?
241. What is a sparse table, and when is it better than a segment tree?
242. What is an LRU cache, and how do you implement O(1) get and put (hash map + doubly linked list)?
243. How would you implement an LFU cache with O(1) operations?
244. What is a Bloom filter, how do you choose its size and number of hash functions, and why can't it have false negatives?
245. What are counting Bloom filters and cuckoo filters?
246. What is Count-Min Sketch, and how does it estimate frequencies in a stream?
247. What is HyperLogLog, and how does it estimate distinct counts with tiny memory?
248. What is a Merkle tree, and how do Git, blockchains, and databases use it?
249. What is consistent hashing, and how does a ring with virtual nodes work as a data structure?
250. What are persistent (immutable) data structures, and how does structural sharing work?
251. What are ropes and gap buffers, and why do text editors use them?
252. What is a quadtree or geohash, and how do they support spatial queries?
253. What is an interval tree, and what queries does it support?

## 18. Advanced Algorithms

254. What is the sweep-line technique, and which problems does it solve?
255. What is meet-in-the-middle, and how does it cut exponential search time?
256. What is bitmask DP, and how does it solve the traveling salesman problem for small n?
257. What is digit DP?
258. What is DP on trees, and how do you compute answers for every root (rerooting)?
259. What is maximum flow, and how do Ford-Fulkerson and Edmonds-Karp work?
260. How does bipartite matching relate to max flow?
261. What are P, NP, NP-complete, and NP-hard, and why does it matter in practice?
262. What are approximation algorithms and heuristics, and when do you use them for NP-hard problems?
263. What are randomized algorithms (Las Vegas vs Monte Carlo)?
264. Problem: LRU Cache.
265. Problem: LFU Cache.
266. Problem: Range Sum Query – Mutable (segment tree or Fenwick tree).
267. Problem: Count of Smaller Numbers After Self.
268. Problem: The Skyline Problem.
269. Problem: Sliding Window Median.
270. Problem: Minimum Interval to Include Each Query.
271. Problem: Shortest Path Visiting All Nodes (BFS + bitmask).

---

# Part D — Senior+ & Staff (DSA in Real Systems)

## 19. Data Structures Behind Real Systems

272. Why do databases use B+ trees for indexes, and why do write-heavy stores use LSM trees?
273. How does an LSM tree use memtables, SSTables, Bloom filters, and compaction?
274. How do inverted indexes work in search engines?
275. How do vector databases use graph-based structures (HNSW) for approximate nearest-neighbor search?
276. Which data structures does Redis use internally (skip lists, hash tables, listpacks, radix trees)?
277. How do rate limiters use token buckets and sliding-window counters?
278. How do timer wheels (hierarchical timing wheels) schedule millions of timeouts efficiently?
279. How do schedulers use priority queues and red-black trees (Linux CFS)?
280. How do garbage collectors use graph traversal (mark-and-sweep, tri-color marking)?
281. How do version control systems use DAGs and content hashing?
282. How do network routers use tries for longest-prefix matching?
283. How does a write-ahead log or ring buffer support high-throughput logging (LMAX Disruptor)?

## 20. Practical Performance & Engineering Judgment

284. Why do cache-friendly data structures often beat theoretically better ones?
285. When does Big-O mislead you, and how do you benchmark properly instead?
286. How do you choose a data structure for a hot path in a production service?
287. How do you design an algorithm that handles data larger than memory (streaming, external sorting, sharding)?
288. How do you find top-K or distinct counts over a stream of billions of events in bounded memory?
289. How do you trade exactness for memory or speed (probabilistic data structures), and how do you explain that trade-off to a product team?
290. How do concurrent data structures (lock-free queues, concurrent hash maps) differ from their single-threaded versions?
291. How would you detect duplicates among a billion URLs with limited memory?
292. How would you design a leaderboard supporting rank queries for millions of users?
293. How would you implement autocomplete over 100 million queries with low latency?

## 21. Interview Approach & Communication

294. How do you approach an unfamiliar problem in an interview (clarify, examples, brute force, optimize, code, test)?
295. How do you recognize which pattern a problem belongs to (two pointers, sliding window, BFS, DP, heap, Union-Find)?
296. How do you choose good test cases, including edge cases (empty input, duplicates, overflow, single element)?
297. How do you communicate trade-offs between multiple solutions?
298. What do you do when you get stuck partway through a problem?
299. How do you verify code correctness without running it (dry runs, invariants)?
300. How do senior and staff interviews evaluate DSA differently from junior interviews?
