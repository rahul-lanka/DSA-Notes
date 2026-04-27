# Count aMaze Paths for Every Direction

## Problem Statement
Given two integers `N` and `M` representing rows and columns of a maze, count total paths from top-left cell to bottom-right cell.

Allowed moves:

- horizontal (left/right)
- vertical (up/down)
- diagonal (all 4 diagonal directions)

Important rule:
- no cell can be visited twice in the same path

Print the total number of valid paths.

## Example 1
Input

```text
2 2
```

Output

```text
5
```

Explanation:
There are 5 valid paths from `(0,0)` to `(1,1)` when all 8 directions are allowed and revisits are not allowed.

## Example 2
Input

```text
1 2
```

Output

```text
1
```

Explanation:
Only one direct move is possible.

## Constraints
- `1 <= N <= 9`
- `1 <= M <= 9`

## Intuition
From any cell, we can try all 8 directions.

For each valid next cell:
- move there
- count paths recursively
- backtrack to explore other choices

Since revisiting is forbidden, we must track visited cells.

This is classic DFS + backtracking on a grid.

## How to Recognize Recursion
This problem is recursive because:

1. Count from current cell depends on counts from next reachable cells.
2. Each move reduces remaining search space.
3. We must explore all valid possibilities.

State:

`countPaths(i, j, visited)`

Meaning:
- currently at cell `(i, j)`
- `visited` tracks cells in current path only

## Algorithm
1. Start DFS from `(0, 0)`.
2. If current cell is destination, return `1`.
3. Mark current cell as visited.
4. Try all 8 directions:
   - compute next cell
   - if inside grid and unvisited, recurse
5. Sum all recursive results.
6. Unmark current cell (backtrack).
7. Return sum.

## Why This Works
Any valid path from current cell must go through one of valid unvisited neighbors.

So total paths from current cell is:

`sum(paths from each valid next neighbor)`

`visited` ensures:
- no cycles
- no repeated cells in same path

Backtracking (`visited[i][j] = false`) restores state so other branches are explored correctly.

## Base Cases
```java
if (i == n - 1 && j == m - 1) {
    return 1;
}
```

Meaning:
- destination reached -> one complete valid path found

Invalid moves are filtered in loop by boundary and visited checks.

## Java Code
```java
import java.util.*;

public class Main {

    static int[][] DIRECTIONS = {
        {0, 1},   // right
        {0, -1},  // left
        {1, 0},   // down
        {-1, 0},  // up
        {1, 1},   // down-right
        {-1, -1}, // up-left
        {1, -1},  // down-left
        {-1, 1}   // up-right
    };

    static int countAllPath(int n, int m) {
        boolean[][] visited = new boolean[n][m];
        return helper(0, 0, n, m, visited);
    }

    static int helper(int i, int j, int n, int m, boolean[][] visited) {
        if (i == n - 1 && j == m - 1) {
            return 1;
        }

        visited[i][j] = true;
        int count = 0;

        for (int[] dir : DIRECTIONS) {
            int ni = i + dir[0];
            int nj = j + dir[1];

            if (ni >= 0 && nj >= 0 && ni < n && nj < m && !visited[ni][nj]) {
                count += helper(ni, nj, n, m, visited);
            }
        }

        visited[i][j] = false; // backtrack
        return count;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        System.out.println(countAllPath(n, m));
    }
}
```

## JavaScript Code
```javascript
function countAllPath(n, m) {
  const visited = Array.from({ length: n }, () => Array(m).fill(false));

  const directions = [
    [0, 1],   // right
    [0, -1],  // left
    [1, 0],   // down
    [-1, 0],  // up
    [1, 1],   // down-right
    [-1, -1], // up-left
    [1, -1],  // down-left
    [-1, 1]   // up-right
  ];

  function helper(i, j) {
    if (i === n - 1 && j === m - 1) {
      return 1;
    }

    visited[i][j] = true;
    let count = 0;

    for (const [di, dj] of directions) {
      const ni = i + di;
      const nj = j + dj;

      if (ni >= 0 && nj >= 0 && ni < n && nj < m && !visited[ni][nj]) {
        count += helper(ni, nj);
      }
    }

    visited[i][j] = false; // backtrack
    return count;
  }

  return helper(0, 0);
}
```

## Dry Run (`N = 2, M = 2`)
Grid cells:
- `(0,0)` start
- `(1,1)` destination

From `(0,0)`, valid first moves are:
- `(0,1)`
- `(1,0)`
- `(1,1)` (diagonal)

Each branch continues using unvisited valid neighbors only.
Total count obtained is `5`.

## Recursion Pattern
```text
count(i, j):
  if destination -> 1
  mark visited
  total = 0
  for each of 8 directions:
    if next is valid and unvisited:
      total += count(next)
  unmark visited
  return total
```

## Time & Space Complexity
- Let `K = N * M` (total cells).
- Time Complexity: exponential in `K` because we explore many simple paths.
- A loose upper bound is `O(8^K)` for branching view.
- Space Complexity: `O(K)` for recursion stack + visited path state.

## Common Mistakes
1. Forgetting to unmark visited during backtracking.
2. Marking visited too late.
3. Allowing out-of-bounds indices.
4. Accidentally revisiting cells and creating cycles.

## Revision Tips
1. This is DFS + backtracking, not simple right/down recursion.
2. Use 8-direction array to avoid repetitive code.
3. Base case: destination returns `1`.
4. No revisit rule needs `visited` matrix.
5. Count problem means sum recursive results.

## Interview Insight
Whenever a grid question says:
- move in all directions
- avoid revisiting cells
- count all valid paths

the default pattern is:

`DFS + visited + backtracking`
