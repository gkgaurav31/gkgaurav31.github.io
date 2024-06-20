---
layout: post
title: Find Minimum in Rotated Sorted Array
date: 2022-12-04 13:39 +0530
author: "Gaurav Kumar"
tags: "java binary_search search neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

- [4,5,6,7,0,1,2] if it was rotated 4 times.
- [0,1,2,4,5,6,7] if it was rotated 7 times.

Given the sorted rotated array nums of unique elements, return the minimum element of this array.
You must write an algorithm that runs in O(log n) time.

[leetcode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

## SOLUTION

The minimum element will be at the "pivot". So, we just need to find out the pivot index using Binary Search.

```java
class Solution {

    public int findMin(int[] nums) {

        int l = 0;
        int r = nums.length-1;

        while(l<=r){

            int mid = (l+r)/2;

            //If the current number is less than previous one, the current index must be the pivot
            if(mid >= 1 && nums[mid] < nums[mid-1]){
                return nums[mid];
            }

            //If the current number is less than the first number in the array, we need to check on left side of current index
            if(nums[mid] < nums[0]){
                r=mid-1;
            //else check on right side of current index
            }else{
                l=mid+1;
            }

        }

        //If we don't find any pivot, it must be sorted
        return nums[0];

    }
}
```

## ANOTHER WAY TO CODE

```java
class Solution {

    public int findMin(int[] nums) {
        int n = nums.length;  // Get the length of the array

        int l = 0;
        int r = n-1;

        // Assume the first element is the minimum initially
        // This will also handle the case in which the array is not actually rotated
        int res = nums[0];

        while(l <= r){

            int m = (l + r) / 2;

            // Check if the middle element is less than the current result
            if(nums[m] < res){

                res = nums[m];  // Update the result with the new minimum

                // Check if the middle element is less than the first element
                // This means the minimum is to the left (including the middle element)
                if(nums[m] < nums[0]){
                    r = m - 1;  // Move the right pointer to just before the middle element
                }else{
                    l = m + 1;  // Move the left pointer to just after the middle element
                }

            }else{

                // If the middle element is not less than the current result
                // Determine the side to search next

                // If the middle element is less than the first element,
                // the smallest value is to the left of middle (including the middle)
                if(nums[m] < nums[0]){
                    r = m - 1;  // Move the right pointer to just before the middle element
                }else{
                    l = m + 1;  // Move the left pointer to just after the middle element
                }
            }
        }

        return res;
    }
}
```
