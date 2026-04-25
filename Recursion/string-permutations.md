# String Permutations

## Problem Statement
You are given a string `str`.

Print all unique permutations of `str` in lexicographic (dictionary) order.

## Example 1
Input

```text
abc
```

Output

```text
abc
acb
bac
bca
cab
cba
```

## Example 2
Input

```text
ab
```

Output

```text
ab
ba
```

## Constraints
- `1 <= |str| <= 5`

## Intuition
At each step, we choose one character and place it in the current answer.

Then we solve the same problem for the remaining characters.

So if current string is `ques`:
- pick one character `ques[i]`
- append it to `asf` (answer so far)
- recurse on `ques` without that character

This is classic backtracking recursion.

## How to Recognize Recursion
Use recursion when:

1. We need to generate all arrangements/combinations.
2. Each choice reduces the problem size.
3. We can define state as:
   - remaining input
   - answer built so far

State:

`permutationPrint(ques, asf)`

Meaning:
- `ques` = characters still left to place
- `asf` = permutation prefix already formed

## Algorithm
1. Start with `ques = sorted(str)` and `asf = ""`.
2. If `ques` is empty, print `asf` and return.
3. Create a set `usedAtLevel` to avoid duplicate choices at the same recursion depth.
4. Loop `i` from `0` to `ques.length - 1`:
   - let `ch = ques[i]`
   - if `ch` already used at this level, skip
   - mark `ch` as used at this level
   - create `remaining = ques[0...i-1] + ques[i+1...]`
   - recurse on `permutationPrint(remaining, asf + ch)`

## Why This Works
Every permutation is built by fixing one character at each position.

At a given depth:
- each distinct character should be chosen once
- `usedAtLevel` prevents duplicate branches for repeated characters

Sorting initially and iterating left to right ensures dictionary order.

So we get:
- all valid permutations
- no duplicates
- lexicographic order

## Base Case
```java
if (ques.length() == 0) {
    System.out.println(asf);
    return;
}
```

Meaning:
- no characters left -> one complete permutation formed

## Java Code
```java
import java.util.*;

public class Main {

    static void permutationPrint(String ques, String asf) {
        if (ques.length() == 0) {
            System.out.println(asf);
            return;
        }

        HashSet<Character> usedAtLevel = new HashSet<>();

        for (int i = 0; i < ques.length(); i++) {
            char ch = ques.charAt(i);

            if (usedAtLevel.contains(ch)) {
                continue; // skip duplicate branch at same depth
            }

            usedAtLevel.add(ch);

            String left = ques.substring(0, i);
            String right = ques.substring(i + 1);
            String remaining = left + right;

            permutationPrint(remaining, asf + ch);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine().trim();
        char[] arr = str.toCharArray();
        Arrays.sort(arr); // for lexicographic order
        permutationPrint(new String(arr), "");
    }
}
```

## JavaScript Code
```javascript
class Solution {
  solve(input) {
    const str = input.trim().split("").sort().join(""); // lexicographic order

    function permutationPrint(ques, asf) {
      if (ques.length === 0) {
        console.log(asf);
        return;
      }

      const usedAtLevel = new Set();

      for (let i = 0; i < ques.length; i++) {
        const ch = ques[i];

        if (usedAtLevel.has(ch)) continue; // skip duplicate branch
        usedAtLevel.add(ch);

        const remaining = ques.substring(0, i) + ques.substring(i + 1);
        permutationPrint(remaining, asf + ch);
      }
    }

    permutationPrint(str, "");
  }
}
```

## Dry Run (`str = "abc"`)
Sorted string is `"abc"`.

At depth 0:
- choose `a` -> solve `"bc"` with `"a"`
- choose `b` -> solve `"ac"` with `"b"`
- choose `c` -> solve `"ab"` with `"c"`

From branch `"a"`:
- choose `b` -> `"ab"` then `c` -> `abc`
- choose `c` -> `"ac"` then `b` -> `acb`

From branch `"b"`:
- `bac`, `bca`

From branch `"c"`:
- `cab`, `cba`

Output:
`abc acb bac bca cab cba`

## Duplicate Case Insight (`str = "aab"`)
Sorted input: `"aab"`

At first level:
- choose first `a` (allowed)
- choose second `a` (skipped by `usedAtLevel`)
- choose `b`

So duplicate branches are removed early.

Unique output:
`aab`
`aba`
`baa`

## Recursion Tree (`abc`)
```text
perm("abc","")
├── a -> perm("bc","a")
│   ├── b -> perm("c","ab")
│   │   └── c -> "abc"
│   └── c -> perm("b","ac")
│       └── b -> "acb"
├── b -> perm("ac","b")
│   ├── a -> "bac"
│   └── c -> "bca"
└── c -> perm("ab","c")
    ├── a -> "cab"
    └── b -> "cba"
```

## Time & Space Complexity
- Number of permutations for distinct characters: `n!`
- Time Complexity: `O(n * n!)` (building strings and printing all permutations)
- Space Complexity: `O(n)` recursion depth (excluding output)

With duplicates, total outputs become:

`n! / (f1! * f2! * ... )`

where `f1, f2, ...` are frequencies of repeated characters.

## Revision Tips
1. State: `f(remaining, answerSoFar)`.
2. Base case: `remaining.length == 0` -> print answer.
3. Loop over all choices in `remaining`.
4. Remove chosen char and recurse.
5. For duplicate chars, use `usedAtLevel`.
6. For lexicographic order, sort input first.

## Interview Insight
If interviewer asks follow-ups:

1. "How do you avoid duplicates?" -> use `usedAtLevel` set at every recursion depth.
2. "How do you print in sorted order?" -> sort initial string and iterate left to right.
3. "Can this be optimized?" -> output size itself is factorial, so generation is inherently expensive.
