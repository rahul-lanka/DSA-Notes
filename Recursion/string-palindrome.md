# String Palindrome

## Problem Statement
Given a string `str`, check whether it is a palindrome.

A palindrome reads the same from left to right and right to left.
Print:
1. `YES` if palindrome
2. `NO` otherwise

## Intuition
Compare characters from both ends:
1. First with last
2. Second with second-last
3. Continue moving inward

If any pair mismatches, it is not a palindrome.

## How to Recognize Recursion
Use recursion when:
1. The same check repeats on a smaller range.
2. Current answer depends on outer characters and inner substring.
3. There is a clear base condition (`start >= end`).

State can be represented as `f(str, start, end)`.

## Algorithm
1. Start with `start = 0`, `end = str.length - 1`.
2. If `start >= end`, return `YES` (all checks passed).
3. If `str[start] != str[end]`, return `NO`.
4. Recurse for inner substring: `f(str, start + 1, end - 1)`.

## Java Code
```java
import java.util.Scanner;

public class Main {
    static String isPalindrome(String str, int start, int end) {
        if (start >= end) {
            return "YES";
        }

        if (str.charAt(start) != str.charAt(end)) {
            return "NO";
        }

        return isPalindrome(str, start + 1, end - 1);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(isPalindrome(str, 0, str.length() - 1));
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

function isPalindrome(str, start, end) {
  if (start >= end) {
    return "YES";
  }

  if (str[start] !== str[end]) {
    return "NO";
  }

  return isPalindrome(str, start + 1, end - 1);
}

rl.on("line", (input) => {
  const str = input.trim();
  console.log(isPalindrome(str, 0, str.length - 1));
  rl.close();
});
```

## Dry Run (`str = "abba"`)
`f("abba", 0, 3)`
1. `str[0] == str[3]` -> `a == a` -> recurse `f("abba", 1, 2)`
2. `str[1] == str[2]` -> `b == b` -> recurse `f("abba", 2, 1)`
3. `start >= end` -> return `YES`

Final output: `YES`

## Recursion Tree (`str = "abba"`)
```text
f("abba", 0, 3)
   |
   v
f("abba", 1, 2)
   |
   v
f("abba", 2, 1) -> YES
```

For non-palindrome example `abca`:
`f("abca", 1, 2)` checks `b` vs `c` -> mismatch -> `NO`.

## Time & Space Complexity
1. Time: `O(n)` (at most `n/2` comparisons).
2. Recursion stack space: `O(n)` in worst case.

## Revision Tips
1. Compare mirror indices: `start` and `end`.
2. Base case: `start >= end` means palindrome.
3. One mismatch is enough to return `NO`.
4. Recursive move: `start + 1`, `end - 1`.
5. Same logic can be written iteratively with two pointers.
