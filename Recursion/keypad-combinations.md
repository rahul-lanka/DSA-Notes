# Keypad Combinations

## Problem Statement
You are given a string `str` containing digits only. Each digit represents a key on a mobile keypad:

- `0 -> .;`
- `1 -> abc`
- `2 -> def`
- `3 -> ghi`
- `4 -> jkl`
- `5 -> mno`
- `6 -> pqrs`
- `7 -> tu`
- `8 -> vwx`
- `9 -> yz`

Print all possible strings that can be formed using the given digit string.

## Example 1
Input

```text
78
```

Output

```text
tv
tw
tx
uv
uw
ux
```

## Example 2
Input

```text
1
```

Output

```text
a
b
c
```

## Constraints
- `0 <= str.length <= 10`
- `str` contains digits only

## Intuition
For every digit, we have a small set of possible characters.

So for each position in the string:
- pick one possible character for the current digit
- add it to the answer built so far
- recursively solve for the next digit

This is a classic recursion and backtracking pattern where:
- `index` tells us which digit we are processing
- `ans` stores the combination built so far

## How to Recognize Recursion
This problem is recursive because:

1. We solve the same kind of problem again and again for the remaining digits.
2. At each step, we make a choice from the characters mapped to the current digit.
3. Once all digits are processed, one complete answer is ready to print.

The recursive state is:

`printKPC(str, ans, index)`

This means:
- process digit at `index`
- current combination formed so far is `ans`

## Algorithm
1. Create a keypad mapping array for digits `0` to `9`.
2. If `index == str.length()`, it means all digits are processed.
3. Print `ans` and return.
4. Otherwise, get the current digit.
5. Find the mapped string for that digit.
6. Loop through every character in that mapped string.
7. Append that character to `ans` and recursively call for `index + 1`.

## Why This Works
For each digit, we try every possible character choice.

If the input is `78`:
- `7 -> tu`
- `8 -> vwx`

So:
- choose `t`, then combine it with `v`, `w`, `x`
- choose `u`, then combine it with `v`, `w`, `x`

That generates all valid combinations:
`tv, tw, tx, uv, uw, ux`

The recursion explores all branches of this choice tree.

## Base Case
The base case is:

```java
if (index == str.length()) {
    System.out.println(ans);
    return;
}
```

This means we have already chosen one character for every digit, so one full combination is ready.

## Java Code
```java
import java.util.*;

public class Main {

    static void printKPC(String ques, String ans, int index) {
        String[] keypad = {
            ".;", "abc", "def", "ghi", "jkl",
            "mno", "pqrs", "tu", "vwx", "yz"
        };

        if (index == ques.length()) {
            System.out.println(ans);
            return;
        }

        int digit = ques.charAt(index) - '0';
        String chars = keypad[digit];

        for (int i = 0; i < chars.length(); i++) {
            printKPC(ques, ans + chars.charAt(i), index + 1);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        printKPC(str, "", 0);
    }
}
```

## JavaScript Code
```javascript
const keypad = [
  ".;",
  "abc",
  "def",
  "ghi",
  "jkl",
  "mno",
  "pqrs",
  "tu",
  "vwx",
  "yz"
];

function printKPC(str) {
  function helper(index, current) {
    if (index === str.length) {
      console.log(current);
      return;
    }

    const digit = Number(str[index]);
    const chars = keypad[digit];

    for (const ch of chars) {
      helper(index + 1, current + ch);
    }
  }

  helper(0, "");
}
```

## Dry Run (`str = "78"`)
`printKPC("78", "", 0)`

- `index = 0`, digit = `7`, chars = `"tu"`

First branch:
- choose `t`
- call `printKPC("78", "t", 1)`
- now digit = `8`, chars = `"vwx"`
- choose `v` -> print `tv`
- choose `w` -> print `tw`
- choose `x` -> print `tx`

Second branch:
- choose `u`
- call `printKPC("78", "u", 1)`
- choose `v` -> print `uv`
- choose `w` -> print `uw`
- choose `x` -> print `ux`

## Time & Space Complexity
- Time Complexity: `O(k^n)` in general, where `n` is the number of digits and `k` is the average number of characters per digit.
- In this keypad, each digit has at most `4` characters, so worst-case time is `O(4^n)`.
- Space Complexity: `O(n)` recursion stack depth, ignoring output storage.

## Revision Tips
1. This is a combinations problem, so think "choice at every index".
2. Recursive state should track:
   - current index
   - answer formed so far
3. Base case comes when all digits are used.
4. For the current digit, loop over all mapped characters.
5. Make recursive call for the next index with updated answer.
6. This pattern is very similar to other recursion problems that build strings step by step.

## Interview Insight
This is a standard recursion and backtracking pattern.

Whenever a problem says:
- generate all possible strings
- generate all combinations
- choose one option from each position

you should immediately think:
- recursive tree
- `ans-so-far`
- `index`
