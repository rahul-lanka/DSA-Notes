Maximum of Array
You are given an array arr of length n. You have to find the maximum element of the array.

Note

You have to use Recursion.

Input Format
The first line of input contains an integer n, size of the array.

The next line contains n space seperated integers denoting the elements of the array.

Output Format
Print single integer representing maximum of the given array

Example 1
Input

3
2 3 10
Output

10
Explanation

10 is maximum among 2,3 and 10.

Example 2
Input

4
1 3 5 7
Output

7
Explanation

7 is maximum among 1,3,5 and 7.


## Java Solution 
import java.util.*;

class Solution {

    public static int findMax(int[] arr, int n) {

        // Base case
        if (n == 1) {
            return arr[0];
        }

        // Recursive call
        int maxOfRest = findMax(arr, n - 1);

        // Compare last element with max of remaining
        return Math.max(arr[n - 1], maxOfRest);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int[] arr = new int[n];

        for(int i = 0; i < n; i++){
            arr[i] = sc.nextInt();
        }

        int result = findMax(arr, n);

        System.out.println(result);
    }
}

## JavaScript Solution
function findMax(arr, n) {

    // Base case
    if (n === 1) {
        return arr[0];
    }

    // Recursive call
    const maxOfRest = findMax(arr, n - 1);

    // Compare last element with remaining
    return Math.max(arr[n - 1], maxOfRest);
}


// Example usage
const arr = [2, 3, 10];
const n = arr.length;

console.log(findMax(arr, n));


## Dry Run 
How Recursion Works

For:

arr = [2,3,10]

Calls happen like this:

findMax(arr,3)
→ max(10 , findMax(arr,2))

findMax(arr,2)
→ max(3 , findMax(arr,1))

findMax(arr,1)
→ return 2

Backtracking:

max(3,2) = 3
max(10,3) = 10



## Time & Space Complexity

Time

O(n)

Space (recursion stack)

O(n)