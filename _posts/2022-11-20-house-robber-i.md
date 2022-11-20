---
layout: post
title: House Robber I
date: 2022-11-20 20:25 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 leetcode dynamic_programming"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.  

[leetcode](https://leetcode.com/problems/house-robber/)

### Solution

```java
class Solution {
    public int rob(int[] nums) {
        
        int n = nums.length;

        if(n == 1) return nums[0];

        int[] dp = new int[n];

        //init
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for(int i=2; i<n; i++){
            //If we rob the current house, we cannot rob the previous one, so we can take total till i-2 buildings
            //If we do not rob the current house, we can take the total till i-1 buildings
            dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1]);
        }

        return dp[n-1];

    }
}
```
