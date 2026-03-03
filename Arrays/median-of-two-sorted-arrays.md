<!-- https://leetcode.com/problems/median-of-two-sorted-arrays/description/?envType=problem-list-v2&envId=array -->

/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */

 ## Brute force code (merge + sort)
 ⏱ Time: O((m+n)log(m+n))
📦 Space: O(m+n)
var findMedianSortedArrays = function(nums1, nums2) {
    const mergedArr = [...nums1, ...nums2].sort((a,b) => a-b);
    const n = mergedArr.length;

    if(n%2 === 1){
        return mergedArr[Math.floor(n/2)];
    }else{
        const mid1 = mergedArr[n / 2];
        const mid2 = mergedArr[n / 2 - 1];
        return (mid1 + mid2) / 2
    }
};

## Better brute force (merge without sorting)
⏱ Time: O(m+n)
📦 Space: O(m+n)
var findMedianSortedArrays = function(nums1, nums2) {
    let i = 0, j = 0;
    let mergedArr = [];

    while( i < nums1.length && j < nums2.length){
        if(nums1[i] < nums2[j]){
            mergedArr.push(nums1[i++]);
        }else{
            mergedArr.push(nums2[j++]);
        }
    }

    while(i < nums1.length) mergedArr.push(nums1[i++]);
    while(j < nums2.length) mergedArr.push(nums2[j++]);

    const n = mergedArr.length;
    if(n % 2 === 1){
        return mergedArr[Math.floor(n/2)];
    }else{
        return (mergedArr[n/2] + mergedArr[n/2 - 1]) / 2;
    }


    
};

## Optimal Approach
⏱ Time: O(log(m+n))
📦 Space: O(1)

var findMedianSortedArrays = function(nums1, nums2) {
    // Ensure nums1 is the smaller array
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }

    let m = nums1.length;
    let n = nums2.length;

    let left = 0;
    let right = m;

    while (left <= right) {
        let partitionX = Math.floor((left + right) / 2);
        let partitionY = Math.floor((m + n + 1) / 2) - partitionX;

        let maxLeftX = partitionX === 0 ? -Infinity : nums1[partitionX - 1];
        let minRightX = partitionX === m ? Infinity : nums1[partitionX];

        let maxLeftY = partitionY === 0 ? -Infinity : nums2[partitionY - 1];
        let minRightY = partitionY === n ? Infinity : nums2[partitionY];

        if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
            if ((m + n) % 2 === 0) {
                return (
                    Math.max(maxLeftX, maxLeftY) +
                    Math.min(minRightX, minRightY)
                ) / 2;
            } else {
                return Math.max(maxLeftX, maxLeftY);
            }
        } else if (maxLeftX > minRightY) {
            right = partitionX - 1;
        } else {
            left = partitionX + 1;
        }
    }
};





## How to remember this in interviews
“We binary search on the smaller array and adjust partitions until the left side max is less than or equal to the right side min.”

"We binary search on the smaller array to find a partition such that the left half contains half the total elements and the maximum element on the left is less than or equal to the minimum element on the right. Once this condition is satisfied, we compute the median from boundary elements."

## How To Recognize This Is The Solution?

When a problem says:

Two sorted arrays

Need something about median

Must run in O(log n)

## Immediately think:

“Binary search on partition.”

Then remember 3 steps:

Always binary search on smaller array

Ensure left side size = (m+n+1)/2

Check boundary condition

That’s the entire algorithm.