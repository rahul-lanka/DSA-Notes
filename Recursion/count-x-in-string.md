## Problem
Count X in a string
Count recursively the total number of character x that appear in the given string.

Input Format
Only line contains the string in which we have to count character x.

Output Format
Print the number of x in string in a single line.

Example 1
Input

abcxxabc
Output

2 
Explanation

2 'x' are there in the given string.

Example 2
Input

addthe
Output

0
Explanation

No x is there in the given string.

Constraints
1 <= Len(str) <= 1000
string contains lowercase letter only.


## Java Solution

import java.util.Scanner;

public class Main {

    static int countX(String str) {

        // base case
        if (str.length() == 0) {
            return 0;
        }

        int count = 0;

        if (str.charAt(0) == 'x') {
            count = 1;
        }

        return count + countX(str.substring(1));
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();

        System.out.println(countX(str));
    }
}

## JavaScript Solution
function countX(str) {

    // base case
    if (str.length === 0) {
        return 0;
    }

    let count = 0;

    if (str[0] === 'x') {
        count = 1;
    }

    return count + countX(str.slice(1));
}

let str = "abcxxabc";
console.log(countX(str));


## Dry Run
How Recursion Works

countX("abcxxabc") → 'a' → 0 + countX("bcxxabc")
countX("bcxxabc")  → 'b' → 0 + countX("cxxabc")
countX("cxxabc")   → 'c' → 0 + countX("xxabc")
countX("xxabc")    → 'x' → 1 + countX("xabc")
countX("xabc")     → 'x' → 1 + countX("abc")
countX("abc")      → 'a' → 0 + countX("bc")
countX("bc")       → 'b' → 0 + countX("c")
countX("c")        → 'c' → 0 + countX("")
countX("")         → 0

## Time & Space Complexity

Time

O(n)

Space (recursion stack)

O(n)

## Revision Tips



## Java Version (Better Recursive Solution)
import java.util.Scanner;

public class Main {

    static int countX(String str, int index) {

        // base case
        if (index == str.length()) {
            return 0;
        }

        int count = 0;

        if (str.charAt(index) == 'x') {
            count = 1;
        }

        return count + countX(str, index + 1);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();

        System.out.println(countX(str, 0));
    }
}

## JavaScript (Better Recursive Solution)
function countX(str, index = 0) {

    // base case
    if (index === str.length) {
        return 0;
    }

    let count = 0;

    if (str[index] === 'x') {
        count = 1;
    }

    return count + countX(str, index + 1);
}

let str = "abcxxabc";
console.log(countX(str));


## Dry Run
countX("abcxxabc",0) → a → 0 + countX(...,1)
countX("abcxxabc",1) → b → 0 + countX(...,2)
countX("abcxxabc",2) → c → 0 + countX(...,3)
countX("abcxxabc",3) → x → 1 + countX(...,4)
countX("abcxxabc",4) → x → 1 + countX(...,5)
countX("abcxxabc",5) → a → 0 + countX(...,6)
countX("abcxxabc",6) → b → 0 + countX(...,7)
countX("abcxxabc",7) → c → 0 + countX(...,8)
countX("abcxxabc",8) → base case → 0

## Interview Insight
Interview Insight
Bad approach:
substring()
slice()

Better approach:
use index pointer

Because it avoids extra memory allocations.

