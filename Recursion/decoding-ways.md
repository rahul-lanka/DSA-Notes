# Decoding Ways

## Problem Statement
You are given a string `str` containing digits only.

You have to decode it using the mapping:

- `1 -> a`
- `2 -> b`
- `3 -> c`
- ...
- `25 -> y`
- `26 -> z`

Your task is to count the total number of valid decodings of the string.

If no decoding is possible, print `0`.

## Example 1
Input

```text
123
```

Output

```text
3
```

Explanation:
- `1 2 3 -> abc`
- `1 23 -> aw`
- `12 3 -> lc`

## Example 2
Input

```text
013
```

Output

```text
0
```

Explanation:
The string starts with `0`, so it is invalid.

## Example 3
Input

```text
303
```

Output

```text
0
```

Explanation:
- `3 0 3` is invalid because `0` has no mapping
- `30 3` is invalid because `30` has no mapping
- `3 03` is invalid because `03` is not allowed

## Constraints
- `0 <= str.length <= 10`
- `str` contains digits only

## Intuition
At every index, we can try:

1. taking one digit
2. taking two digits

But only if the chosen number forms a valid character.

So from the current index:
- if the current digit is `0`, this path is invalid
- otherwise, take one digit and recurse
- if the next two digits form a number from `10` to `26`, also take two digits and recurse

The answer is the total count of all valid recursive branches.

## How to Recognize Recursion
This problem is recursive because:

1. The answer for the whole string depends on answers for smaller suffixes.
2. At each index, we make one or two valid choices.
3. The total answer is the sum of results from those choices.

The recursive state is:

`countWays(str, index)`

This means:
- count the number of valid decodings starting from `index`

## Algorithm
1. Define a recursive function `helper(index)`.
2. If `index == str.length`, return `1` because one valid decoding is completed.
3. If `str[index] == '0'`, return `0` because decoding cannot start with `0`.
4. Count the ways by taking one digit:
   - `count = helper(index + 1)`
5. If there are at least two digits left:
   - form the two-digit number
   - if it lies between `10` and `26`, add `helper(index + 2)` to the count
6. Return `count`

## Why This Works
Every valid decoding must begin in exactly one of these ways:

- take one digit
- take two digits, if valid

So if we count all valid decodings from both possibilities and add them, we get the total answer.

Invalid branches naturally return `0`, so they do not affect the final result.

## Base Cases
There are two important base cases:

```java
if (index == str.length()) {
    return 1;
}

if (str.charAt(index) == '0') {
    return 0;
}
```

Meaning:
- reaching the end means one valid decoding has been formed
- encountering `0` at the current position means the path is invalid

## Java Code
```java
import java.util.*;

public class Main {

    static int countWays(String str, int index) {
        if (index == str.length()) {
            return 1;
        }

        if (str.charAt(index) == '0') {
            return 0;
        }

        int count = countWays(str, index + 1);

        if (index + 1 < str.length()) {
            int num = Integer.parseInt(str.substring(index, index + 2));

            if (num >= 10 && num <= 26) {
                count += countWays(str, index + 2);
            }
        }

        return count;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(countWays(str, 0));
    }
}
```

## JavaScript Code
```javascript
function countWays(str) {
  function helper(index) {
    if (index === str.length) {
      return 1;
    }

    if (str[index] === "0") {
      return 0;
    }

    let count = helper(index + 1);

    if (index + 1 < str.length) {
      const num = Number(str.substring(index, index + 2));

      if (num >= 10 && num <= 26) {
        count += helper(index + 2);
      }
    }

    return count;
  }

  return helper(0);
}
```

## Optimized Approach: Memoization (Top-Down DP)
The plain recursive solution recalculates the same suffix many times.

For example, while solving `"1234"`, the recursion may reach the same `index` from different paths and solve that suffix again.

To avoid repeated work, we store the answer for each `index` in a memo object.

So:
- if we have already computed the result for an index, return it directly
- otherwise, compute it once, store it, and reuse it later

This changes the time complexity from exponential to linear.

## JavaScript Memoization Code
```javascript
var numDecodings = function(s) {
  const memo = {};

  function helper(index) {
    if (index === s.length) return 1;

    if (s[index] === "0") return 0;

    if (memo[index] !== undefined) return memo[index];

    let count = helper(index + 1);

    if (index + 1 < s.length) {
      const num = Number(s.substring(index, index + 2));

      if (num >= 10 && num <= 26) {
        count += helper(index + 2);
      }
    }

    memo[index] = count;
    return count;
  }

  return helper(0);
};
```

## Why Memoization Helps
Without memoization:
- the same suffix can be solved many times
- recursion tree keeps repeating work

With memoization:
- each index is solved only once
- later calls reuse the stored answer

The state is still:

`helper(index)`

But now its result is cached in:

`memo[index]`

## Time & Space Complexity of Memoization
- Time Complexity: `O(n)` because each index is computed at most once.
- Space Complexity: `O(n)` for the memo object and recursion stack.

## Dry Run (`str = "123"`)
`countWays("123", 0)`

At `index = 0`:
- current digit is `1`
- take one digit -> solve for `"23"`
- take two digits (`12`) -> solve for `"3"`

First branch:
- `helper(1)` for `"23"`
- from `2`, take one digit -> `"3"`
- from `2`, take two digits (`23`) -> end

Second branch:
- `helper(2)` for `"3"`
- take one digit -> end

So valid paths are:
- `1 | 2 | 3`
- `1 | 23`
- `12 | 3`

Total = `3`

## Invalid Case Insight
For `str = "013"`:
- first character is `0`
- immediately invalid
- answer = `0`

For `str = "303"`:
- `3 | 03` is invalid
- `30 | 3` is invalid
- `3 | 0 | 3` is invalid
- answer = `0`

## Time & Space Complexity
- Time Complexity: `O(2^n)` in the worst case because each index can branch into two recursive calls.
- Space Complexity: `O(n)` due to recursion stack depth.

## Revision Tips
1. At each index, there are at most two choices: take one digit or take two digits.
2. `0` can never start a valid decoding.
3. If you reach the end, return `1` because one valid path is completed.
4. Two-digit numbers are valid only from `10` to `26`.
5. Final answer is the sum of all valid recursive branches.
6. This is a count-recursion problem, so recursive calls return numbers.
7. If overlapping subproblems appear, use memoization to optimize from recursion to top-down DP.

## Interview Insight
This is a classic recursion-on-index problem.

Whenever you see:
- a string or array
- choices made from the current index
- answer required as count of valid ways

you should think of:

`solve(index) = answer for suffix starting at index`
