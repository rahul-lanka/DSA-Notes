## Problem 
Ways to write n as sum
Difficulty: MediumAccuracy: 34.53%Submissions: 25K+Points: 4Average Time: 30m
Given a positive integer n, the task is to find the number of different ways in which n can be written as a sum of two or more positive integers. Return the answer with the modulo 109+7.

Examples:

Input: n = 5
Output: 6
Explanation: 1+1+1+1+1, 1+1+1+2, 1+1+3, 1+4, 2+1+2 and 2+3. So, a total of 6 ways.
Input: n = 3
Output: 2
Explanation: 1+1+1 and 1+2.  So, a total of 2 ways.
Expected Time Complexity: O(n^2)
Expected Auxiliary Space: O(n)

Constraints:
1 <= n <= 103


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

