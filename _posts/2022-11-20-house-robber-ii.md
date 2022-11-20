---
layout: post
title: House Robber II
date: 2022-11-20 20:23 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 leetcode dynamic_programming"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.  

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.  

[leetcode](https://leetcode.com/problems/house-robber-ii/description/)

### Solution

```java
class Solution {
    
    public int rob(int[] nums) {

        int n = nums.length;
        if(n == 1) return nums[0];
        if(n == 2) return Math.max(nums[0],nums[1]);

        //rob first house
        //so the last hour cannot be robbed
        int a = robHelper(nums, 0, n-2);

        //don't rob first house
        //so the last house can be robbed
        int b = robHelper(nums, 1, n-1);

        return Math.max(a, b);
        
    }

    public int robHelper(int[] nums, int start, int end){

        int n = nums.length;

        int[] dp = new int[n];
        
        dp[start] = nums[start];
        dp[start+1] = Math.max(nums[start+1], nums[start]);

        for(int k=start+2; k<=end; k++){
            dp[k] = Math.max(dp[k-1], dp[k-2] + nums[k]);
        } 

        return dp[end];       
    }
    
}
```
