---
layout: post
title: Find First and Last Position of Element in Sorted Array
date: 2023-08-18 22:49 +0530
author: "Gaurav Kumar"
tags: "java binary_search leetcode leetcode150"
categories: "binary_search"
---

## Problem Description

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.
If target is not found in the array, return [-1, -1].
You must write an algorithm with O(log n) runtime complexity.

[leetcode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Solution

Use binary search to find the starting index of the target. Then, run the binary search again but this time looking for the last occurance of the element.

```java
class Solution {

    public int[] searchRange(int[] nums, int target) {

        int n = nums.length;

        // Use binary search to find the starting and ending positions of the target value.
        int startIdx = searchStart(nums, target); // Find the starting position of the target.
        int endIdx = searchEnd(nums, target); // Find the ending position of the target.

        // If the starting index is not found, the target is not present in the array.
        if(startIdx == -1) return new int[]{-1, -1};

        // Return the found starting and ending indices.
        return new int[]{startIdx, endIdx};
    }

    // Binary search function to find the starting position of the target value.
    public int searchStart(int[] nums, int target){

        int n = nums.length;

        int l = 0;
        int r = n - 1;

        int idx = -1;

        // Perform binary search while left pointer is less than or equal to right pointer.
        while(l <= r){
            int m = (l + r) / 2;

            if(nums[m] == target){
                idx = m; // Update the index for the starting position.
                r = idx - 1; // Move the right pointer to the left to look for a better start index if possible.
            }
            else if(nums[m] > target)
                r = m - 1; // Adjust the right pointer to narrow down the search range.
            else
                l = m + 1; // Adjust the left pointer to narrow down the search range.
        }

        return idx;
    }

    // Binary search function to find the ending position of the target value.
    public int searchEnd(int[] nums, int target){

        int n = nums.length;

        int l = 0;
        int r = n - 1;

        int idx = -1;

        // Perform binary search while left pointer is less than or equal to right pointer.
        while(l <= r){

            int m = (l + r) / 2;

            if(nums[m] == target){
                idx = m; // Update the index for the ending position.
                l = m + 1; // Move the left pointer to the right to look for a better end index if possible.
            }
            else if(nums[m] > target)
                r = m - 1; // Adjust the right pointer to narrow down the search range.
            else
                l = m + 1; // Adjust the left pointer to narrow down the search range.

        }

        return idx;
    }
}
```

### SMALL OPTIMIZATION

- If the first binary search returns -1, that means that the target does not exist in the array and we can return -1
- If we do find the start index, we can use that to optimize the second binary search. We know that the array is sorted. So, while searching for the endIndex, we can set the "LEFT" (start of the search area) for binary search as startIdx which we found in first step. Refer to the code to see how that would look like. This may not help if the startIdx is some initial index like 0,1,2 etc.

```java
class Solution {

    public int[] searchRange(int[] nums, int target) {

        int n = nums.length;

        int startIdx = searchStart(nums, target);
        if(startIdx == -1) return new int[]{-1, -1};

        int endIdx = searchEnd(startIdx, nums, target);

        return new int[]{startIdx, endIdx};

    }

    public int searchStart(int[] nums, int target){

        int n = nums.length;

        int l = 0;
        int r = n-1;

        int idx = -1;

        while(l<=r){

            int m = (l+r)/2;

            if(nums[m] == target){
                idx = m;
                r = idx-1;
            }
            else if(nums[m] > target)
                r = m - 1;
            else
                l = m + 1;

        }

        return idx;

    }

    public int searchEnd(int left, int[] nums, int target){

        int n = nums.length;

        int l = left; //the endIndex will be minimum startIdx we would have found
        int r = n-1;

        int idx = -1;

        while(l<=r){

            int m = (l+r)/2;

            if(nums[m] == target){
                idx = m;
                l = m + 1;
            }
            else if(nums[m] > target)
                r = m - 1;
            else
                l = m + 1;

        }

        return idx;

    }

}
```
