# Count String Encodings

## Problem Statement
You are given a string `str` containing digits only.

You have to decode it using the mapping:

- `1 -> a`
- `2 -> b`
- `3 -> c`
- ...
- `25 -> y`
- `26 -> z`

Your task is to count the total number of valid encodings of the string.

If no encoding is possible, print `0`.

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
At every position, we can try:

1. taking one digit
2. taking two digits

But only if the chosen number forms a valid encoding.

So from any index:
- if current digit is `0`, no encoding is possible from there
- otherwise, take one digit and recurse
- if the next two digits form a number from `10` to `26`, also take two digits and recurse

This makes recursion natural because the remaining work is always:

`count valid encodings from the next index`

## How to Recognize Recursion
This problem is recursive because:

1. The problem breaks into smaller subproblems on the remaining suffix of the string.
2. At each index, we make one or two valid choices.
3. The final answer is the sum of all valid choices from the current position.

The recursive state is:

`countEncodings(str, index)`

This means:
- count the number of valid encodings starting from `index`

## Algorithm
1. Define a recursive function `helper(index)`.
2. If `index == str.length`, one complete valid encoding is formed, so return `1`.
3. If `str[index] == '0'`, return `0` because encoding cannot start with `0`.
4. Count the encodings by taking one digit:
   - `count = helper(index + 1)`
5. If there are at least two digits left:
   - form the 2-digit number
   - if it lies between `10` and `26`, add `helper(index + 2)` to the count
6. Return the final count.

## Why This Works
At every valid index, we explore every legal way to decode the current part of the string:

- one-digit decode
- two-digit decode, if allowed

Since every encoding must start with one of these two possibilities, summing both recursive results gives the total number of valid encodings.

If a path becomes invalid, such as starting with `0`, it returns `0` and contributes nothing.

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
- reaching the end means we built one valid encoding
- encountering `0` at the current index means this path is invalid

## Java Code
```java
import java.util.*;

public class Main {

    static int countEncodings(String str, int index) {
        if (index == str.length()) {
            return 1;
        }

        if (str.charAt(index) == '0') {
            return 0;
        }

        int count = countEncodings(str, index + 1);

        if (index + 1 < str.length()) {
            int num = Integer.parseInt(str.substring(index, index + 2));

            if (num >= 10 && num <= 26) {
                count += countEncodings(str, index + 2);
            }
        }

        return count;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(countEncodings(str, 0));
    }
}
```

## JavaScript Code
```javascript
function countEncodings(str) {
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

## Dry Run (`str = "123"`)
`countEncodings("123", 0)`

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
- Time Complexity: `O(2^n)` in the worst case because from each index we may branch into two recursive calls.
- Space Complexity: `O(n)` due to recursion stack depth.

## Revision Tips
1. At each index, only two choices are possible: take one digit or take two digits.
2. `0` can never start a valid encoding.
3. If you reach the end, return `1`, not `0`, because one valid decoding path is completed.
4. Two-digit numbers are valid only from `10` to `26`.
5. Final answer is the sum of all valid recursive branches.
6. This is a count-recursion problem, so recursive calls return numbers, not strings.

## Interview Insight
This is a classic recursion-on-index problem.

Whenever you see:
- digit string
- valid 1-step or 2-step choices
- total number of ways

you should think:
- recursive branching
- invalid path returns `0`
- completed path returns `1`
