<!-- https://leetcode.com/problems/two-sum/description/?envType=problem-list-v2&envId=array -->

## Difficulty
Easy

## Pattern
Hashing

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */


## Brute Force Approach
- Use two loops
- Time: O(n^2)
- Space: O(1)

var twoSum = function(nums, target) {
    for(let i = 0; i < nums.length; i++){
        for(let j = i+1; j< nums.length; j++){
            if(nums[i] + nums[j] === target){
                return [i,j];
            }
        }
    }
};

## Optimized Approach
- Use HashMap
- Time: O(n)
- Space: O(n)

var twoSum = function(nums, target) {
    const map = new Map();
    for(let i = 0; i < nums.length; i++){
        let need = target - nums[i];

        if(map.has(need)){
            return [map.get(need), i];
        }

        map.set(nums[i], i);
    }
};


## Java Solution
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();

        for(int i = 0; i < nums.length; i++){
        int need = target - nums[i];
        if(map.containsKey(need)){
            return new int[] {map.get(need), i};
        }
        map.put(nums[i], i);

        }
        return new int[] {};
    }
}

## Why Hashing is Perfect Here
Instead of checking: “Is there another number that adds up to target?”
We convert it to: “Have I already seen the number I need?”
It converts:O(n2)→O(n)