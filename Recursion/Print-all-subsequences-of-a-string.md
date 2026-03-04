# Print All Subsequences of a String

## Problem Statement
Given a string `str`, print all subsequences of it.

A subsequence is formed by deleting zero or more characters without changing the order of the remaining characters.

Example: For `abc`, subsequences are:
`abc ab ac a bc b c ""` (including empty subsequence).

## Intuition
For every character, you only have 2 choices:
1. Include it in the current answer.
2. Exclude it.

So each character creates a binary decision, which naturally forms recursion.

## How to Recognize Recursion
Use recursion when:
1. Problem asks to generate all combinations/subsequences/subsets.
2. Each step has a clear "take or not take" choice.
3. You can define a smaller subproblem by removing one character.

Here, `f(remainingString, currentAnswer)` depends on a smaller remaining string.

## Algorithm
1. Start with `remaining = str`, `ans = ""`.
2. Base case: if `remaining` is empty, print `ans` and return.
3. Recursive case:
   1. Include first char: `f(remaining[1...], ans + remaining[0])`
   2. Exclude first char: `f(remaining[1...], ans)`
4. This solves the follow-up too (no index parameter needed).

## Java Code
```java
import java.util.Scanner;

public class Main {
    static void printSubsequence(String s, String ans) {
        if (s.length() == 0) {
            System.out.print("\"" + ans + "\" ");
            return;
        }

        char ch = s.charAt(0);
        String rest = s.substring(1);

        // include current character
        printSubsequence(rest, ans + ch);

        // exclude current character
        printSubsequence(rest, ans);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        printSubsequence(s, "");
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

function printSubsequence(s, ans) {
  if (s.length === 0) {
    process.stdout.write(`"${ans}" `);
    return;
  }

  const ch = s[0];
  const rest = s.slice(1);

  // include current character
  printSubsequence(rest, ans + ch);

  // exclude current character
  printSubsequence(rest, ans);
}

rl.on("line", (input) => {
  printSubsequence(input.trim(), "");
  rl.close();
});
```

## Dry Run (`str = "abc"`)
`f("abc", "")`
1. Include `a` -> `f("bc", "a")`
   1. Include `b` -> `f("c", "ab")`
      1. Include `c` -> print `"abc"`
      2. Exclude `c` -> print `"ab"`
   2. Exclude `b` -> `f("c", "a")`
      1. Include `c` -> print `"ac"`
      2. Exclude `c` -> print `"a"`
2. Exclude `a` -> `f("bc", "")`
   1. Include `b` -> `f("c", "b")`
      1. Include `c` -> print `"bc"`
      2. Exclude `c` -> print `"b"`
   2. Exclude `b` -> `f("c", "")`
      1. Include `c` -> print `"c"`
      2. Exclude `c` -> print `""`

## Recursion Tree (`abc`)
```text
                         ("abc","")
                        /          \
                 include a        exclude a
                  ("bc","a")      ("bc","")
                  /      \          /      \
          include b   exclude b  include b  exclude b
           ("c","ab") ("c","a")  ("c","b") ("c","")
            /    \      /   \      /   \      /   \
         "abc" "ab"  "ac" "a"   "bc" "b"   "c"  ""
```

## Time & Space Complexity
1. Number of subsequences = `2^n`.
2. Time:
   1. Recursion calls: `O(2^n)`
   2. Printing/output work included: `O(n * 2^n)` (total characters across all outputs)
3. Auxiliary space (recursion stack): `O(n)`.

## Revision Tips
1. Keyword to trigger approach: "all subsequences" -> "include/exclude recursion".
2. State design: `f(remaining, ans)` (no index needed).
3. Base case: when `remaining` is empty, output `ans`.
4. Two recursive calls only:
   1. take current char
   2. skip current char
5. Total outputs are always `2^n` (including empty subsequence).
