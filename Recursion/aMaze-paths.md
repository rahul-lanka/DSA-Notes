# aMaze Paths

## Problem Statement
Given two integers `N` and `M` representing the number of rows and columns in a maze, print all paths from the top-left cell to the bottom-right cell.

Only two moves are allowed:

- `h` = move one step right
- `v` = move one step down

You must print all valid paths.

The horizontal recursive call should be made before the vertical recursive call, so output order stays correct.

## Example 1
Input

```text
2
2
```

Output

```text
hv
vh
```

Explanation:
- `hv` means right, then down
- `vh` means down, then right

## Example 2
Input

```text
1
2
```

Output

```text
h
```

Explanation:
There is only one possible move: go right once.

## Constraints
- `1 <= N <= 10`
- `1 <= M <= 10`

## Intuition
At every cell, we have at most two choices:

1. move right
2. move down

We keep building the path string while moving toward the destination.

So:
- if we reach the destination, print the path
- if we go outside the maze, stop that path
- otherwise, try horizontal first, then vertical

## How to Recognize Recursion
This problem is recursive because:

1. From the current cell, the remaining problem is the same type of problem.
2. Each choice creates a smaller subproblem.
3. We need to generate all valid paths, not just count them.

The recursive state can be thought of as:

`aMazePaths(i, j, path)`

This means:
- currently standing at cell `(i, j)`
- `path` stores the moves taken so far

## Algorithm
1. Start from the top-left cell with an empty path string.
2. If current cell is the destination, print the path and return.
3. If current cell goes outside the maze, return.
4. Make the horizontal recursive call first:
   - move right
   - append `"h"` to path
5. Then make the vertical recursive call:
   - move down
   - append `"v"` to path

## Why This Works
Every valid path from start to destination must begin with one of these moves:

- go right
- go down

By recursively exploring both possibilities from every valid cell, we generate every possible path.

Invalid paths automatically stop when they move outside the maze.

Since we call horizontal first and then vertical, the output order also matches the requirement.

## Base Cases
There are two important base cases:

```java
if (i == n - 1 && j == m - 1) {
    System.out.println(path);
    return;
}

if (i >= n || j >= m) {
    return;
}
```

Meaning:
- destination reached -> print path
- outside maze -> invalid path, stop recursion

## Java Code
```java
import java.util.Scanner;

public class Main {

    static void aMazePaths(int n, int m, int i, int j, String path) {
        if (i == n - 1 && j == m - 1) {
            System.out.println(path);
            return;
        }

        if (i >= n || j >= m) {
            return;
        }

        aMazePaths(n, m, i, j + 1, path + "h");
        aMazePaths(n, m, i + 1, j, path + "v");
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        aMazePaths(n, m, 0, 0, "");
    }
}
```

## JavaScript Code
```javascript
function printMazePaths(n, m) {
  function helper(i, j, path) {
    if (i === n - 1 && j === m - 1) {
      console.log(path);
      return;
    }

    if (i >= n || j >= m) {
      return;
    }

    helper(i, j + 1, path + "h");
    helper(i + 1, j, path + "v");
  }

  helper(0, 0, "");
}
```

## Dry Run (`N = 2`, `M = 2`)
Start at `(0, 0)` with `path = ""`

From `(0, 0)`:
- move right -> `(0, 1)` with `"h"`
- move down -> `(1, 0)` with `"v"`

First branch:
- `(0, 1, "h")`
- move right -> outside maze, stop
- move down -> `(1, 1, "hv")` -> print `hv`

Second branch:
- `(1, 0, "v")`
- move right -> `(1, 1, "vh")` -> print `vh`
- move down -> outside maze, stop

Output order:
`hv`
`vh`

## Recursion Tree (`N = 2`, `M = 2`)
```text
helper(0,0,"")
├── h -> helper(0,1,"h")
│   ├── h -> outside
│   └── v -> helper(1,1,"hv") -> print
└── v -> helper(1,0,"v")
    ├── h -> helper(1,1,"vh") -> print
    └── v -> outside
```

## Time & Space Complexity
- Time Complexity: `O(2^(N+M))` as an upper bound for recursive exploration.
- Space Complexity: `O(N + M)` due to recursion stack and path length.
- Since all paths are printed, actual work also depends on total output size.

## Revision Tips
1. At each cell, only two choices are possible: `h` or `v`.
2. Destination base case prints the path.
3. Out-of-bounds base case returns immediately.
4. Horizontal call first, vertical call second.
5. This is a classic "generate all paths" recursion problem.
6. Build the answer using `path + "h"` and `path + "v"`.

## Interview Insight
This problem teaches a very common recursion pattern:

- current position
- available moves
- base case for success
- base case for invalid path
- build answer while exploring

Whenever a question asks to print all possible paths in a grid, think:

`solve(currentRow, currentCol, pathSoFar)`
