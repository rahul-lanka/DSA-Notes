# Number of Ways to Form Natural Number

## Problem Statement
Given an integer `N`, count how many ways it can be represented as a sum of **unique natural numbers**.

Each natural number can be used at most once.
Order does not matter.

Example:
1. `6 = 1 + 2 + 3`
2. `6 = 1 + 5`
3. `6 = 2 + 4`
4. `6 = 6`

So for `N = 6`, the answer is `4`.

## Intuition
This is a subset-style recursion problem.

For every number `num`, we have only 2 choices:
1. Include `num` in the sum
2. Exclude `num` from the sum

Then move to the next number: `num + 1`.

So the recursive state can be represented as:
`countWays(remaining, num)`

Meaning:
Count the number of ways to form `remaining` using unique numbers starting from `num`.

## How to Recognize Recursion
Use recursion when:
1. A problem can be broken into smaller problems of the same type.
2. Each step has a clear decision branch.
3. You need to explore all valid combinations.

Here:
1. Include current number -> solve smaller sum
2. Exclude current number -> keep sum same, move ahead

That gives the recursion:
1. `countWays(remaining - num, num + 1)`
2. `countWays(remaining, num + 1)`

## Algorithm
1. Start with `countWays(N, 1)`.
2. If `remaining == 0`, one valid combination is found, return `1`.
3. If `remaining < 0`, return `0`.
4. If `num > remaining`, return `0` because larger numbers cannot help now.
5. Recursively calculate:
   1. Include current number
   2. Exclude current number
6. Return sum of both results.

## Why This Algorithm Works
This problem is really asking:
"How many subsets of numbers from `1` to `N` have sum `N`?"

That is why the include/exclude method fits perfectly.

For every current number `num`, there are only 2 valid decisions:
1. Take it in the current combination
2. Skip it and move ahead

These 2 choices are enough to generate every possible subset exactly once.

Why exactly once?
1. We always move forward from `num` to `num + 1`
2. So a number is never reused
3. And order never changes

That means:
1. `1 + 5` is counted
2. `5 + 1` is not counted again separately

So recursion explores all valid unique combinations without duplicates.

## How the Recursion Moves
Suppose current state is:
`countWays(remaining, num)`

This means:
We still need to form `remaining`, and the next number we are allowed to consider is `num`.

Now there are 2 branches:

1. Include branch
If we use `num`, then:
1. Remaining sum becomes `remaining - num`
2. Next allowed number becomes `num + 1`

So the recursive call is:
`countWays(remaining - num, num + 1)`

2. Exclude branch
If we skip `num`, then:
1. Remaining sum stays the same
2. Next allowed number still becomes `num + 1`

So the recursive call is:
`countWays(remaining, num + 1)`

This is how the recursion systematically checks every possibility.

## Why the Base Cases Make Sense
1. `remaining == 0`
We formed the target sum exactly.
So this is one valid way, and we return `1`.

2. `remaining < 0`
We crossed the target sum.
This path is invalid, so return `0`.

3. `num > remaining`
The current number is already bigger than what is left.
Since future numbers will be even larger, no valid sum can be formed from this path.
So return `0`.

## Java Code
```java
import java.util.Scanner;

public class Main {
    static int countWays(int remaining, int num) {
        if (remaining == 0) {
            return 1;
        }

        if (remaining < 0 || num > remaining) {
            return 0;
        }

        int include = countWays(remaining - num, num + 1);
        int exclude = countWays(remaining, num + 1);

        return include + exclude;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(countWays(n, 1));
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

function countWays(remaining, num) {
  if (remaining === 0) {
    return 1;
  }

  if (remaining < 0 || num > remaining) {
    return 0;
  }

  const include = countWays(remaining - num, num + 1);
  const exclude = countWays(remaining, num + 1);

  return include + exclude;
}

rl.on("line", (input) => {
  const n = Number(input.trim());
  console.log(countWays(n, 1));
  rl.close();
});
```

## Dry Run (`N = 6`)
`countWays(6, 1)`

1. Include `1` -> `countWays(5, 2)`
   1. Include `2` -> `countWays(3, 3)`
      1. Include `3` -> `countWays(0, 4)` -> `1`
      2. Exclude `3` -> `countWays(3, 4)` -> `0`
   2. Exclude `2` -> `countWays(5, 3)`
      1. Include `3` -> `countWays(2, 4)` -> `0`
      2. Exclude `3` -> continue further
2. Exclude `1` -> `countWays(6, 2)`
   1. Include `2` -> `countWays(4, 3)`
   2. Exclude `2` -> `countWays(6, 3)`

Valid combinations found:
1. `1 + 2 + 3`
2. `1 + 5`
3. `2 + 4`
4. `6`

Final output: `4`

## Small Example of How It Thinks
Take `N = 4`.

Start:
`countWays(4, 1)`

1. Include `1` -> `countWays(3, 2)`
   1. Include `2` -> `countWays(1, 3)` -> invalid
   2. Exclude `2` -> `countWays(3, 3)`
      1. Include `3` -> `countWays(0, 4)` -> one valid way: `1 + 3`
2. Exclude `1` -> `countWays(4, 2)`
   1. Include `2` -> `countWays(2, 3)` -> invalid
   2. Exclude `2` -> `countWays(4, 3)`
      1. Include `3` -> `countWays(1, 4)` -> invalid
      2. Exclude `3` -> `countWays(4, 4)`
         1. Include `4` -> `countWays(0, 5)` -> one valid way: `4`

So total ways for `4` are:
1. `1 + 3`
2. `4`

## Recursion Tree (`N = 6`)
```text
countWays(6, 1)
├── include 1 -> countWays(5, 2)
│   ├── include 2 -> countWays(3, 3)
│   │   ├── include 3 -> countWays(0, 4) -> 1
│   │   └── exclude 3 -> countWays(3, 4) -> 0
│   └── exclude 2 -> countWays(5, 3)
└── exclude 1 -> countWays(6, 2)
    ├── include 2 -> countWays(4, 3)
    └── exclude 2 -> countWays(6, 3)
```

The full recursion tree is larger, but every branch follows the same include/exclude pattern.

## Time & Space Complexity
1. Time complexity: `O(2^N)` in the worst case because each number creates 2 branches.
2. Recursion stack space: `O(N)`.

## Revision Tips
1. Think of it as choosing a subset of numbers from `1` to `N`.
2. Every number has 2 choices: include or exclude.
3. Base case: `remaining == 0` means one valid way is found.
4. Stop when `remaining < 0` or `num > remaining`.
5. Use `num + 1` so each natural number is used at most once.
