# Maze Problem (With Multi-Step Jumps)

## Problem Statement
You are given `n` rows and `m` columns of a maze.

Start from top-left and reach bottom-right.

In one move, you can jump:

- horizontally: `h1`, `h2`, ...
- vertically: `v1`, `v2`, ...
- diagonally: `d1`, `d2`, ...

Print all valid paths.

## Example 1
Input

```text
2
2
```

Output

```text
h1v1
v1h1
d1
```

## Example 2
Input

```text
3
3
```

Output

```text
h1h1v1v1
h1h1v2
h1v1h1v1
h1v1v1h1
h1v1d1
h1v2h1
h1d1v1
h2v1v1
h2v2
v1h1h1v1
v1h1v1h1
v1h1d1
v1h2v1
v1v1h1h1
v1v1h2
v1d1h1
v2h1h1
v2h2
d1h1v1
d1v1h1
d1d1
d2
```

## Constraints
- `1 <= n <= 5`
- `1 <= m <= 5`

## Intuition
At any cell `(sr, sc)`, we have three categories of moves:

1. horizontal jumps of size `1..(dc - sc)`
2. vertical jumps of size `1..(dr - sr)`
3. diagonal jumps of size `1..min(dr - sr, dc - sc)`

For each jump size, recurse to the new cell and append move token (`h2`, `v1`, `d3`, etc.) to the path string.

This is recursive path generation with variable move lengths.

## How to Recognize Recursion
This problem is recursive because:

1. From current cell, remaining task is same type: reach destination.
2. Each move leads to smaller subproblem.
3. We must print all possibilities.

State:

`printMazePaths(sr, sc, dr, dc, path)`

Meaning:
- `sr, sc`: current cell
- `dr, dc`: destination cell
- `path`: moves taken so far

## Algorithm
1. If current cell is destination, print `path` and return.
2. Try all horizontal jumps:
   - for `ms = 1` while `sc + ms <= dc`
   - recurse to `(sr, sc + ms)` with `path + "h" + ms`
3. Try all vertical jumps:
   - for `ms = 1` while `sr + ms <= dr`
   - recurse to `(sr + ms, sc)` with `path + "v" + ms`
4. Try all diagonal jumps:
   - for `ms = 1` while `sr + ms <= dr` and `sc + ms <= dc`
   - recurse to `(sr + ms, sc + ms)` with `path + "d" + ms`

## Why This Works
Any valid path from current cell must begin with one valid jump of type:

- horizontal
- vertical
- diagonal

We exhaust all jump sizes in each category, then recurse.

Since each recursive call moves closer to destination, recursion terminates.
Every valid path is printed exactly once.

## Base Case
```java
if (sr == dr && sc == dc) {
    System.out.println(path);
    return;
}
```

Meaning:
- destination reached -> one complete path formed

## Java Code
```java
import java.util.Scanner;

public class Main {

    static void printMazePaths(int sr, int sc, int dr, int dc, String path) {
        if (sr == dr && sc == dc) {
            System.out.println(path);
            return;
        }

        // Horizontal jumps
        for (int ms = 1; sc + ms <= dc; ms++) {
            printMazePaths(sr, sc + ms, dr, dc, path + "h" + ms);
        }

        // Vertical jumps
        for (int ms = 1; sr + ms <= dr; ms++) {
            printMazePaths(sr + ms, sc, dr, dc, path + "v" + ms);
        }

        // Diagonal jumps
        for (int ms = 1; sr + ms <= dr && sc + ms <= dc; ms++) {
            printMazePaths(sr + ms, sc + ms, dr, dc, path + "d" + ms);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        printMazePaths(1, 1, n, m, "");
    }
}
```

## JavaScript Code
```javascript
function printMazePaths(sr, sc, dr, dc, path) {
  if (sr === dr && sc === dc) {
    console.log(path);
    return;
  }

  // Horizontal jumps
  for (let ms = 1; sc + ms <= dc; ms++) {
    printMazePaths(sr, sc + ms, dr, dc, path + "h" + ms);
  }

  // Vertical jumps
  for (let ms = 1; sr + ms <= dr; ms++) {
    printMazePaths(sr + ms, sc, dr, dc, path + "v" + ms);
  }

  // Diagonal jumps
  for (let ms = 1; sr + ms <= dr && sc + ms <= dc; ms++) {
    printMazePaths(sr + ms, sc + ms, dr, dc, path + "d" + ms);
  }
}

// Example driver
function solve(n, m) {
  printMazePaths(1, 1, n, m, "");
}
```

## Dry Run (`n = 2`, `m = 2`)
Start from `(1,1)` to `(2,2)`.

Possible first jumps:
- `h1` -> then only `v1` remains -> `h1v1`
- `v1` -> then only `h1` remains -> `v1h1`
- `d1` -> directly reaches destination -> `d1`

Output:
`h1v1`
`v1h1`
`d1`

## Recursion Pattern
```text
print(sr, sc, path):
  if destination:
    print(path)
    return

  for each horizontal jump size:
    print(nextHorizontal, path + "h" + jump)

  for each vertical jump size:
    print(nextVertical, path + "v" + jump)

  for each diagonal jump size:
    print(nextDiagonal, path + "d" + jump)
```

## Time & Space Complexity
- Number of recursive calls depends on total number of paths, which grows quickly.
- Time Complexity is output-sensitive (proportional to number of generated paths).
- Recursion stack depth in worst case is `O(n + m)` for small jumps.

## Common Mistakes
1. Forgetting jump-size loops and doing only single-step movement.
2. Wrong loop boundaries for jump size.
3. Mixing 0-based and 1-based indexing in start/destination.
4. Printing before reaching destination.

## Revision Tips
1. State: `f(sr, sc, path)`.
2. Base case: if at destination, print and return.
3. Use three loops: horizontal, vertical, diagonal.
4. Append move token with jump size (`"h" + ms`, etc.).
5. This is "print all paths", so function returns `void`.

## Interview Insight
If interviewer asks:
- "multi-jump maze path"
- "print all paths"
- "h/v/d moves allowed"

default approach is:

`recursive DFS with variable jump loops`
