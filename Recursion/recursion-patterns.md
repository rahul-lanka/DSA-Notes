# Recursion Patterns & Problem Solving Guide

## The Standard Format (How to Approach ANY Recursive Problem)

Whenever you encounter a recursive problem, break it down using this **3-Step Framework**. Memorize this format for your revisions:

1. **Base Case(s):** 
   - *What is the smallest or invalid input?* 
   - This is the condition that stops the recursion from running infinitely. (e.g., `if (n == 0) return 1;`, `if (node == null) return;`)
2. **Recursive Call (Trust the Function):** 
   - *Assume the function already works indefinitely for smaller inputs.* 
   - Call the same function on a smaller chunk of the problem (e.g., `n - 1`, `n / 2`, `node.left`).
3. **Self Work / Combine Result:** 
   - *What do YOU need to do in the current step?* 
   - Combine the result of the recursive call with your current element to form the final answer.

---

## 5 Common Recursion Patterns

### 1. Tail Recursion
- **Description:** The recursive call is the *very last* operation performed in the function. There is zero computation left to do after the recursive call returns. You usually pass an accumulator down the chain.
- **How to Identify:**
  - The problem asks you to accumulate a result linearly (often utilizing helper functions with extra parameters).
  - You process the current item *first*, then pass the updated state down to the next call.
- **Complexity:**
  - **Time:** $O(N)$
  - **Space:** $O(N)$ auxiliary stack space. *(Note: $O(1)$ in languages that support Tail Call Optimization, though Java/Python do not natively optimize this).*
- **Classic Example:** Simple factorial with an accumulator variable, linear array search.

### 2. Head Recursion
- **Description:** The recursive call is the *first* operation. All actual data processing happens *after* the recursive call returns (during the "unwinding" phase of the stack).
- **How to Identify:**
  - You need to process the bottom-most elements first before handling the current element.
  - Typical phrase: "Print a linked list in reverse order" or bottom-up evaluations.
- **Complexity:**
  - **Time:** $O(N)$
  - **Space:** $O(N)$ auxiliary stack space.
- **Classic Example:** Reversing a string or printing elements backwards.

### 3. Tree Recursion (Multiple Branching)
- **Description:** The function makes two or more recursive calls to itself, forming a tree-like execution structure instead of a straight line.
- **How to Identify:**
  - You have overlapping subproblems that branch out. 
  - Standard phrase: "Select or don't select", "Take it or leave it". Often seen in dynamic programming problems.
- **Complexity:**
  - **Time:** Usually Exponential $O(2^N)$ or $O(3^N)$, depending on the number of branches per call.
  - **Space:** $O(N)$ where $N$ is the maximum depth of the recursion tree.
- **Classic Example:** Nth Fibonacci number (`fib(n-1) + fib(n-2)`), Knapsack problems.

### 4. Backtracking
- **Description:** An optimization of Tree Recursion. You explore multiple branch paths, but as soon as a path violates a constraint, you drop it ("prune" the branch), **undo the choice**, and backtrack to try a different decision.
- **How to Identify:**
  - Problems asking for "all possible ways", "valid combinations/permutations", or solving bounded grid puzzles.
  - The flow is: `Make a Choice` -> `Recurse` -> `Undo the Choice`.
- **Complexity:**
  - **Time:** Exponential, often $O(N!)$ or $O(2^N)$. However, it runs substantially faster than basic tree recursion because many invalid branches are skipped.
  - **Space:** $O(N)$ for the recursion depth.
- **Classic Example:** N-Queens, Sudoku Solver, Subsets, Target Sum combinations.

### 5. Divide and Conquer
- **Description:** You split the problem into halves (or multiple smaller chunks), solve each chunk independently via recursion, and then merge the results together.
- **How to Identify:**
  - Problems related to sorting arrays, dealing with Binary Trees, or quickly searching a sorted space.
  - The size of the problem reduces by a fraction (e.g., $N/2$) at each step instead of incrementing down by 1.
- **Complexity:**
  - **Time:** Often $O(N \log N)$ (e.g., merging halves) or $O(\log N)$ (e.g., disregarding one half entirely).
  - **Space:** Usually $O(\log N)$ due to the shallow stack depth.
- **Classic Example:** Merge Sort, Quick Sort, Binary Search, Tree Traversals.

---

## 📝 Quick Revision Cheat Sheet

| Pattern | Identification Trigger | Time Complexity | Space Complexity (Output ignored) |
|---|---|---|---|
| **Tail & Head** | Linear traversal, single choice at a time | $O(N)$ | $O(N)$ depth |
| **Tree Recursion** | Multiple choices ("take vs skip"), branching | $O(2^N)$ | $O(N)$ depth |
| **Backtracking**| Generating "all combinations", paths, mazes | $O(N!)$ or $O(2^N)$ | $O(N)$ depth |
| **Divide & Conquer**| Splitting problem in half (Trees, Sorting array) | $O(N \log N)$ or $O(\log N)$| $O(\log N)$ depth |
