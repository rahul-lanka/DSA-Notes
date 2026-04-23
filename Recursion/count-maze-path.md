# Count Maze Path

##LeetCode Problem - Unique Paths
https://leetcode.com/problems/unique-paths/description/

## Problem Statement

Given two integers `N` and `M` representing the number of rows and columns in a maze, count the total number of paths from the top-left cell to the bottom-right cell.

Only two moves are allowed:

- move one step right
- move one step down

You have to return or print the total number of possible paths.

## Example 1

Input

```text
2
2
```

Output

```text
2
```

Explanation:
There are two valid paths:

- right, then down
- down, then right

## Example 2

Input

```text
1
2
```

Output

```text
1
```

Explanation:
There is only one possible path.

## Constraints

- `1 <= N <= 10`
- `1 <= M <= 10`

## Intuition

At every cell, we can make at most two choices:

1. move right
2. move down

But unlike `aMaze Paths`, here we do not print paths.
We only count how many valid ways exist.

So from any cell:

- if we reach the destination, return `1`
- if we go outside the maze, return `0`
- otherwise, total paths = right paths + down paths

## How to Recognize Recursion

This problem is recursive because:

1. The answer for the current cell depends on smaller subproblems.
2. From each cell, we branch into the same kind of problem.
3. The final answer is the sum of valid results from both choices.

The recursive state is:

`countMazePath(i, j)`

This means:

- count the number of ways to reach destination starting from cell `(i, j)`

## Algorithm

1. Start from `(0, 0)`.
2. If current cell is the destination, return `1`.
3. If current cell goes outside the maze, return `0`.
4. Recursively count:
   - paths by moving right
   - paths by moving down
5. Return the sum of both counts.

## Why This Works

Every valid path from the current cell must start with exactly one of these moves:

- right
- down

So the total number of paths from a cell is:

`rightPaths + downPaths`

If a move goes outside the maze, it contributes `0`.
If a move reaches the destination, it contributes `1`.

This ensures every valid path is counted exactly once.

## Base Cases

There are two important base cases:

```java
if (i == n - 1 && j == m - 1) {
    return 1;
}

if (i >= n || j >= m) {
    return 0;
}
```

Meaning:

- destination reached -> one valid path found
- outside maze -> invalid path

## Java Code

```java
import java.util.Scanner;

public class Main {

    static int countMazePath(int n, int m, int i, int j) {
        if (i == n - 1 && j == m - 1) {
            return 1;
        }

        if (i >= n || j >= m) {
            return 0;
        }

        int rightPaths = countMazePath(n, m, i, j + 1);
        int downPaths = countMazePath(n, m, i + 1, j);

        return rightPaths + downPaths;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        System.out.println(countMazePath(n, m, 0, 0));
    }
}
```

## JavaScript Code

```javascript
function countMazePath(n, m) {
  function helper(i, j) {
    if (i === n - 1 && j === m - 1) {
      return 1;
    }

    if (i >= n || j >= m) {
      return 0;
    }

    return helper(i, j + 1) + helper(i + 1, j);
  }

  return helper(0, 0);
}
```

## Optimized Approach: Memoization (Top-Down DP)

The plain recursive solution solves the same cell again and again.

For example, in a bigger grid, the number of paths from a middle cell may be recalculated from multiple branches.

To avoid repeated work, we store the result for each cell in a memo object.

So:

- if a cell result is already computed, return it directly
- otherwise, compute it once and store it

This reduces the time complexity from exponential to polynomial.

## JavaScript Memoization Code

```javascript
var uniquePaths = function (m, n) {
  const memo = {};

  function helper(i, j) {
    if (i === m - 1 && j === n - 1) return 1;

    if (i >= m || j >= n) return 0;

    const key = i + "," + j;
    if (memo[key] !== undefined) return memo[key];

    memo[key] = helper(i, j + 1) + helper(i + 1, j);
    return memo[key];
  }

  return helper(0, 0);
};
```

## Why Memoization Helps

Without memoization:

- same cell can be solved many times
- recursion tree has overlapping subproblems

With memoization:

- each cell is solved only once
- later calls reuse the stored answer

The state becomes:

`helper(i, j)`

And its answer is cached using:

`memo[i + "," + j]`

## Dry Run (`N = 2`, `M = 2`)

Start from `(0, 0)`

From `(0, 0)`:

- right -> `(0, 1)`
- down -> `(1, 0)`

First branch:

- `(0, 1)`
- right -> outside -> `0`
- down -> `(1, 1)` -> `1`
- total from `(0, 1)` = `1`

Second branch:

- `(1, 0)`
- right -> `(1, 1)` -> `1`
- down -> outside -> `0`
- total from `(1, 0)` = `1`

Total from `(0, 0)`:

- `1 + 1 = 2`

## Recursion Tree (`N = 2`, `M = 2`)

```text
helper(0,0)
├── right -> helper(0,1)
│   ├── right -> outside -> 0
│   └── down  -> helper(1,1) -> 1
└── down  -> helper(1,0)
    ├── right -> helper(1,1) -> 1
    └── down  -> outside -> 0
```

## Time & Space Complexity

- Recursive Time Complexity: `O(2^(N+M))` as an upper bound.
- Recursive Space Complexity: `O(N + M)` due to recursion stack.
- Memoization Time Complexity: `O(N * M)` because each cell is computed once.
- Memoization Space Complexity: `O(N * M)` for memo plus recursion stack.

## Revision Tips

1. At each cell, only two choices are possible: right or down.
2. Destination base case returns `1`.
3. Out-of-bounds base case returns `0`.
4. Count problem means return values are added, not printed.
5. Formula is: `count(i, j) = count(i, j + 1) + count(i + 1, j)`.
6. If the same subproblem repeats, use memoization.

## Interview Insight

This is a very common recursion-to-DP problem.

If the question asks:

- number of ways
- grid movement
- only right/down moves

think:

`ways(currentCell) = ways(rightCell) + ways(downCell)`
