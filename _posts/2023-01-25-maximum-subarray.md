---
layout: post
title: Maximum Subarray
date: 2023-01-25 23:21 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an integer array nums, find the subarray with the largest sum, and return its sum.

[leetcode](https://leetcode.com/problems/maximum-subarray/)

### SOLUTION

```java
class Solution {
    
    public int maxSubArray(int[] nums) {

        int maxSum = nums[0];

        int currentSum = nums[0];

        for(int i=1; i<nums.length; i++){

            //the current number may be more than the previous number plus current number, if the previous sum was negative
            currentSum = Math.max(currentSum+nums[i], nums[i]);

            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
        
    }
}
```
