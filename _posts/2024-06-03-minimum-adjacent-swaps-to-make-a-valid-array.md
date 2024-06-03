---
layout: post
title: Minimum Adjacent Swaps to Make a Valid Array
date: 2024-06-03 11:29 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode amazon"
categories: "arrays"
---

## Problem Description

You are given a 0-indexed integer array nums.

Swaps of adjacent elements are able to be performed on nums.

A valid array meets the following conditions:

- The largest element (any of the largest elements if- there are multiple) is at the rightmost position in the- array.
- The smallest element (any of the smallest elements if there are multiple) is at the leftmost position in the array.

Return the minimum swaps required to make nums a valid array.

## Solution

```java
class Solution {

    public int minimumSwaps(int[] nums) {

        int n = nums.length;

        int minIndex = 0;
        int maxIndex = 0;

        // find left most smallest element
        // find right most largest element
        for(int i = 0; i < n; i++){

            // Update minIndex if a smaller element is found
            if(nums[i] < nums[minIndex]){
                minIndex = i;
            }

            // we need to find right most largest element
            // that is why we take equal here also
            if(nums[i] >= nums[maxIndex]){
                maxIndex = i;
            }

        }

        // If the largest element's index is before the smallest element's index
        // Subtract 1 to account for the overlap of swaps
        // If we keep moving, let's say, smallest element towards left, the largest element will automatically move towards right by 1 step
        // So, we need to handle this case
        if(maxIndex < minIndex){
            return minIndex + (n - maxIndex - 1) - 1;
        }

        // Otherwise, simply push smallest to left and largest to right
        return minIndex + (n - maxIndex - 1);
    }
}
```
