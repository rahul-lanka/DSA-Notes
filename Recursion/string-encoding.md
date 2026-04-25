# String Encodings

## Problem Statement
You are given a digit string `str`.

Decode it using:

- `1 -> a`
- `2 -> b`
- `3 -> c`
- ...
- `25 -> y`
- `26 -> z`

Print all valid encodings, one per line, in lexicographic order.

If no encoding is possible, print nothing.

## Example 1
Input

```text
123
```

Output

```text
abc
aw
lc
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
```

Explanation:
String starts with `0`, so no valid encoding.

## Example 3
Input

```text
303
```

Output

```text
```

Explanation:
- `3 0 3` invalid (`0` has no mapping)
- `30 3` invalid (`30` has no mapping)
- `3 03` invalid (`03` not allowed)

## Constraints
- `0 <= str.length <= 10`
- `str` contains digits only

## Intuition
At each index, we can try:

1. taking one digit
2. taking two digits

Only valid numbers can be decoded.

So from `index`:
- if current digit is `0`, stop this path
- always try one-digit decode (`1` to `9`)
- try two-digit decode only if number is `10` to `26`

Each valid choice appends one character to the answer string and recurses forward.

## How to Recognize Recursion
This is recursion because:

1. Current answer depends on encodings of remaining suffix.
2. Every index branches into one or two smaller subproblems.
3. We must generate all valid outputs, not just count.

State:

`printEncodings(index, ans)`

Meaning:
- `index` = current position in input string
- `ans` = decoded string built so far

## Algorithm
1. Start recursion from `index = 0` with empty answer.
2. If `index == str.length`, print `ans` and return.
3. If `str[index] == '0'`, return (invalid path).
4. Take one digit:
   - convert to number
   - append mapped character
   - recurse for `index + 1`
5. If two digits exist:
   - form `num = str[index..index+1]`
   - if `10 <= num <= 26`, append mapped char
   - recurse for `index + 2`

## Why This Works
Any valid decoding at a position must begin with either:

- one valid digit
- or one valid two-digit number

By exploring both possibilities recursively, we enumerate all valid decodings.

Invalid branches return immediately, so only valid decoded strings are printed.

## Base Cases
```java
if (index == str.length()) {
    System.out.println(ans);
    return;
}

if (str.charAt(index) == '0') {
    return;
}
```

Meaning:
- end reached -> one full valid decoding formed
- `0` encountered -> invalid path

## Java Code
```java
import java.util.Scanner;

public class Main {

    static char toChar(int num) {
        return (char) ('a' + num - 1);
    }

    static void printEncodings(String str, int index, String ans) {
        if (index == str.length()) {
            System.out.println(ans);
            return;
        }

        if (str.charAt(index) == '0') {
            return;
        }

        int one = str.charAt(index) - '0';
        printEncodings(str, index + 1, ans + toChar(one));

        if (index + 1 < str.length()) {
            int two = Integer.parseInt(str.substring(index, index + 2));

            if (two >= 10 && two <= 26) {
                printEncodings(str, index + 2, ans + toChar(two));
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine().trim();
        printEncodings(str, 0, "");
    }
}
```

## JavaScript Code
```javascript
function printEncodings(str) {
  const map = [
    "",
    "a", "b", "c", "d", "e", "f", "g",
    "h", "i", "j", "k", "l", "m", "n",
    "o", "p", "q", "r", "s", "t", "u",
    "v", "w", "x", "y", "z"
  ];

  function helper(index, ans) {
    if (index === str.length) {
      console.log(ans);
      return;
    }

    if (str[index] === "0") return;

    const one = Number(str[index]);
    helper(index + 1, ans + map[one]);

    if (index + 1 < str.length) {
      const two = Number(str.substring(index, index + 2));

      if (two >= 10 && two <= 26) {
        helper(index + 2, ans + map[two]);
      }
    }
  }

  helper(0, "");
}
```

## Lexicographic Order Note
For this mapping, recursion that tries:

1. one-digit branch first
2. two-digit branch second

prints results in lexicographic order for this problem's output pattern.

## Dry Run (`str = "123"`)
`helper(0, "")`

At index `0`:
- one digit `1 -> a` -> `helper(1, "a")`
- two digits `12 -> l` -> `helper(2, "l")`

From `helper(1, "a")`:
- one digit `2 -> b` -> `helper(2, "ab")` -> then `3 -> c` -> print `abc`
- two digits `23 -> w` -> print `aw`

From `helper(2, "l")`:
- one digit `3 -> c` -> print `lc`

Output:
`abc`
`aw`
`lc`

## Invalid Case Insight
For `str = "013"`:
- first char is `0`
- recursion stops immediately
- no output

For `str = "303"`:
- `3 0 3` invalid
- `30 3` invalid
- `3 03` invalid
- no output

## Time & Space Complexity
- Worst-case Time Complexity: `O(2^n)` (branching recursion)
- Recursion Stack Space: `O(n)`
- Output cost is additional and depends on number/length of valid encodings.

## Difference from Decoding Ways
`string-encoding`:
- print all valid decoded strings
- recursive function returns `void`

`decoding-ways`:
- count number of valid decodings
- recursive function returns `int`
- memoization is very useful there for optimization

## Revision Tips
1. At each index, try one-digit and valid two-digit choice.
2. `0` can never start a valid token.
3. Base case for print problems: when end reached, print `ans`.
4. This is "generate all valid strings" recursion.
5. In count variant, same recursion shape but return counts.

## Interview Insight
When prompt says:
- "print all possible decodings"
- mapping `1..26 -> a..z`

think:

`dfs(index, decodedSoFar)`

and branch on one-digit and two-digit choices.
