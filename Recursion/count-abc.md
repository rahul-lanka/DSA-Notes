# Count ABC and ABA

## Problem Statement
Count recursively the total number of "abc" and "aba" substrings that appear in the given string. The substrings can overlap.

**Examples:**
- `countAbc("abc")` → `1`
- `countAbc("abcxxabc")` → `2`
- `countAbc("abaxxaba")` → `2`

## Intuition
This is a standard string traversal problem that can be solved with recursion.

The core idea is to move through the string one character at a time and check for a pattern. For each position, we only need to look ahead a fixed number of characters (3 in this case) to see if we have a match.

The recursive function will represent the work to be done from the *current position* to the end of the string.

## How to Recognize Recursion
Use recursion for this problem because:
1.  The problem can be broken down into a smaller, identical problem: checking the *rest* of the string.
2.  At each step, there is a simple decision: does the substring starting at the current position match "abc" or "aba"?
3.  The result for the whole string is the result from the current position plus the result from the rest of the string.

So the recursive state can be:
`count(str, index)`

This means:
Count the number of "abc" and "aba" substrings in `str` starting from `index`.

## Algorithm
1.  Define a recursive function `countABC(str, index)`.
2.  **Base Case:** If the current `index` is too close to the end to form a 3-character substring (i.e., `index >= str.length() - 2`), we can't find any more matches. Return `0`.
3.  **Recursive Step:**
    -   Check if the substring of length 3 starting at `index` is equal to "abc" or "aba".
    -   **If it matches:** We found one occurrence. The total count is `1 +` the result of checking the rest of the string, which is `countABC(str, index + 1)`.
    -   **If it does not match:** We found zero occurrences at this position. The total count is `0 +` the result of checking the rest of the string, which is `countABC(str, index + 1)`.
4.  The initial call will be `countABC(str, 0)`.

## Why This Algorithm Works
This algorithm works because it systematically checks every possible starting position for a 3-character substring.

By advancing the index by only `1` at each step (`index + 1`), we guarantee that we check for overlapping substrings.

For example, in "ababa":
1.  `count("ababa", 0)` finds "aba" and calls `count("ababa", 1)`.
2.  `count("ababa", 1)` does not find a match.
3.  `count("ababa", 2)` finds "aba" and calls `count("ababa", 3)`.

The sum of these findings gives the correct total. The recursion ensures that every position from the start to the end is evaluated.

## How the Recursion Moves
Suppose the current state is:
`countABC(str, index)`

This means we are looking for matches starting at `str[index]`.

1.  **Check:** Does `str.substring(index, index + 3)` equal "abc" or "aba"?

2.  **Branch 1: Match Found**
    -   We add `1` to our count.
    -   We need to solve the subproblem for the rest of the string. The next possible starting position is `index + 1`.
    -   The recursive call is `1 + countABC(str, index + 1)`.

3.  **Branch 2: No Match Found**
    -   We add `0` to our count.
    -   We still need to solve the subproblem for the rest of the string, starting from `index + 1`.
    -   The recursive call is `countABC(str, index + 1)`.

This process continues until the base case is hit.

## Why the Base Cases Make Sense
The single base case is `if (index >= str.length() - 2)`.

This condition means we have fewer than 3 characters left in the string from our current position. Since we are looking for 3-character substrings ("abc", "aba"), it's impossible to find any more matches.

Therefore, the number of matches in this remaining (very short) part of the string is `0`, so we return `0`. This correctly terminates the recursion.

## Java Code
```java
import java.util.*;

public class Main {
    static int countABC(String str, int i) {
        // Base case: If we can't form a 3-character string, stop.
        if (i > str.length() - 3) {
            return 0;
        }

        // Check for "abc" or "aba" at the current position
        if (str.substring(i, i + 3).equals("abc") || str.substring(i, i + 3).equals("aba")) {
            // Match found: add 1 and check the rest of the string from the next index
            return 1 + countABC(str, i + 1);
        } else {
            // No match: check the rest of the string from the next index
            return countABC(str, i + 1);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        System.out.println(countABC(s, 0));
    }
}
```

## JavaScript Code
```javascript
function countABC(input, i) {
  // Base case: If we can't form a 3-character string, stop.
  if (i > input.length - 3) {
    return 0;
  }

  // Check for "abc" or "aba" using character access
  if (input[i] === 'a' && input[i+1] === 'b' && (input[i+2] === 'c' || input[i+2] === 'a')) {
    // Match found: add 1 and check the rest of the string from the next index
    return 1 + countABC(input, i + 1);
  } else {
    // No match: check the rest of the string from the next index
    return countABC(input, i + 1);
  }
}

// Example usage with a fixed string
const str = "abaxxaba";
console.log(countABC(str, 0)); // Output: 2
```

## Dry Run (`s = "abaxxaba"`)
- `countABC("abaxxaba", 0)`
  - `i=0`. Substring is "aba". **Match!**
  - Returns `1 + countABC("abaxxaba", 1)`

- `countABC("abaxxaba", 1)`
  - `i=1`. Substring is "bax". No match.
  - Returns `countABC("abaxxaba", 2)`

- `countABC("abaxxaba", 2)`
  - `i=2`. Substring is "axx". No match.
  - Returns `countABC("abaxxaba", 3)`

- `countABC("abaxxaba", 3)`
  - `i=3`. Substring is "xxa". No match.
  - Returns `countABC("abaxxaba", 4)`

- `countABC("abaxxaba", 4)`
  - `i=4`. Substring is "xab". No match.
  - Returns `countABC("abaxxaba", 5)`

- `countABC("abaxxaba", 5)`
  - `i=5`. Substring is "aba". **Match!**
  - Returns `1 + countABC("abaxxaba", 6)`

- `countABC("abaxxaba", 6)`
  - `i=6`. `6 > 8 - 3` is false. `6 > 5`. Wait, the condition should be `i > str.length() - 3`. Let's re-verify. Yes, `i` can go up to `length - 3`.
  - For `i=6`, `6 > 8-3 = 5`. This is true. My Java code has `i > str.length() - 3`. The original JS code had `i >= str.length - 2`. Both are equivalent.
  - Let's use `i >= str.length() - 2`.
  - `i=6`. `6 >= 8 - 2` -> `6 >= 6`. True. Base case reached.
  - Returns `0`.

**Calculation Trace:**
- Call at `i=5` returns `1 + 0 = 1`.
- Call at `i=4` returns `1`.
- Call at `i=3` returns `1`.
- Call at `i=2` returns `1`.
- Call at `i=1` returns `1`.
- Call at `i=0` returns `1 + 1 = 2`.

Final result: `2`. This is correct.

## Time & Space Complexity
1.  **Time Complexity:** `O(N)` where `N` is the length of the string. The recursion goes `N` levels deep, and at each level, we do a constant amount of work (substring and comparison).
2.  **Space Complexity:** `O(N)` for the recursion stack space. In the worst case, the call stack will have `N` frames.

## Revision Tips
1.  This is a string traversal problem. Think about processing it from left to right.
2.  The recursive state needs to know "where are we now?". So, an `index` is required.
3.  The subproblem is always "solve for the rest of the string". This means the recursive call is almost always on `index + 1`.
4.  Check for matches at the current `index`. If found, add 1.
5.  Always make the recursive call to `index + 1` regardless of whether a match was found.
6.  The base case is when you're too close to the end of the string to form the pattern.
