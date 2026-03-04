# Print Stair Paths

## Problem Statement
Given an integer `n` (number of stairs), print all paths to reach exactly stair `n` if in one move you can take:
1. `1` step
2. `2` steps
3. `3` steps

Each path should be printed as a string like `121` (take 1 step, then 2, then 1).

## Intuition
From stair `n`, you can try 3 choices:
1. Jump `1` step -> solve for `n - 1`
2. Jump `2` steps -> solve for `n - 2`
3. Jump `3` steps -> solve for `n - 3`

Keep building the path string while moving toward base cases.

## How to Recognize Recursion
Use recursion when:
1. A problem can branch into multiple smaller same-type subproblems.
2. You need to generate all valid combinations/paths.
3. There is a natural stopping condition (`n == 0` or `n < 0`).

Here, `f(n, path)` becomes `f(n-1, path+"1")`, `f(n-2, path+"2")`, `f(n-3, path+"3")`.

## Algorithm
1. Start with `printStairPaths(n, "")`.
2. If `n == 0`, print `path` (valid complete path), return.
3. If `n < 0`, return (invalid path).
4. Recurse in this order:
   1. `n-1` with `path + "1"`
   2. `n-2` with `path + "2"`
   3. `n-3` with `path + "3"`

## Java Code
```java
import java.util.Scanner;

public class Main {
    static void printStairPaths(int n, String path) {
        if (n == 0) {
            System.out.println(path);
            return;
        }

        if (n < 0) {
            return;
        }

        printStairPaths(n - 1, path + "1");
        printStairPaths(n - 2, path + "2");
        printStairPaths(n - 3, path + "3");
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        printStairPaths(n, "");
    }
}
```

## JavaScript Code
```javascript
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

function printStairPaths(n, path) {
  if (n === 0) {
    console.log(path);
    return;
  }

  if (n < 0) {
    return;
  }

  printStairPaths(n - 1, path + "1");
  printStairPaths(n - 2, path + "2");
  printStairPaths(n - 3, path + "3");
}

rl.on("line", (input) => {
  const n = Number(input.trim());
  printStairPaths(n, "");
  rl.close();
});
```

## Dry Run (`n = 3`)
`f(3, "")`
1. Jump `1` -> `f(2, "1")`
   1. Jump `1` -> `f(1, "11")`
      1. Jump `1` -> `f(0, "111")` -> print `111`
      2. Jump `2` -> `f(-1, "112")` -> stop
      3. Jump `3` -> `f(-2, "113")` -> stop
   2. Jump `2` -> `f(0, "12")` -> print `12`
   3. Jump `3` -> `f(-1, "13")` -> stop
2. Jump `2` -> `f(1, "2")`
   1. Jump `1` -> `f(0, "21")` -> print `21`
   2. Jump `2` -> `f(-1, "22")` -> stop
   3. Jump `3` -> `f(-2, "23")` -> stop
3. Jump `3` -> `f(0, "3")` -> print `3`

Output order:
`111, 12, 21, 3`

## Recursion Tree (`n = 3`)
```text
                         f(3, "")
                   /        |        \
              +1 /      +2  |     +3  \
                /            |          \
           f(2,"1")       f(1,"2")    f(0,"3") -> print 3
         /    |    \       /   |   \
  f(1,"11") f(0,"12") f(-1,"13") f(0,"21") f(-1,"22") f(-2,"23")
   / | \      print12    stop      print21    stop       stop
f(0,"111") f(-1,"112") f(-2,"113")
 print111      stop        stop
```

## Time & Space Complexity
1. Recurrence: `T(n) = T(n-1) + T(n-2) + T(n-3) + O(1)`.
2. Time complexity (upper bound): `O(3^n)`.
3. Recursion stack space: `O(n)`.
4. Total output size is variable and can be large, because all valid paths are printed.

## Revision Tips
1. Think: "At each stair, I have 3 choices: 1, 2, 3."
2. Base cases:
   1. `n == 0` -> print path
   2. `n < 0` -> return
3. Recursion order controls output order (`1`, then `2`, then `3`).
4. This is a classic "choices + path building" recursion pattern.
5. Use string accumulation (`path + step`) to track decisions.
