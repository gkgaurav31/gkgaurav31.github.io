---
layout: post
title: Search Insert Position
date: 2023-08-15 17:02 +0530
author: "Gaurav Kumar"
tags: "java BST leetcode leetcode150"
categories: "BST"
---
## PROBLEM DESCRIPTION

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with O(log n) runtime complexity.

[leetcode](https://leetcode.com/problems/search-insert-position/)

## SOLUTION

We can use binary search to solve this. The possible range is [0, n-1] where the target can be inserted. If the target is more than all the elements in the array, the index would be n, which can be initiailize with and look for lower index values.

```java
class Solution {
    
    public int searchInsert(int[] nums, int target) {

        int n = nums.length;

        int l = 0;        
        int r = n - 1;    

        // Initialize idx to be the length of the array, which is the index where target would be inserted if it's larger than all elements
        int idx = n;      

        while (l <= r) {
            
            // Calculate the middle index
            int m = (l + r) / 2;    

            // If the middle element is equal to the target, return its index
            if (nums[m] == target) {
                return m;            
            // Update idx with the current middle index, as target should be inserted before the element at index m
            // Update the right pointer to search in the left half of the current range to check for lower matches (given array is already)
            } else if (nums[m] > target) {
                idx = Math.min(idx, m);   
                r = m - 1;                sorted)
            // Else update the left pointer to search in the right half of the current range
            } else {
                l = m + 1;                
            }

        }

        return idx;  

    }
    
}
```
