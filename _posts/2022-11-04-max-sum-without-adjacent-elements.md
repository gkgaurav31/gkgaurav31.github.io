---
layout: post
title: Max Sum Without Adjacent Elements
date: 2022-11-04 13:49 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.  

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.  

[leetcode](https://leetcode.com/problems/house-robber/description/)

### Solution

```java

class Solution {

    public int rob(int[] nums) {

        if(nums.length == 1) return nums[0];

        //To keep a track of max till ith position
        int[] dp = new int[nums.length];

        //Initialize max for 0th and 1st positions
        dp[0] = nums[0];
        dp[1] = Math.max(dp[0], nums[1]);

        //Iterate through position >=2
        for(int i=2; i<nums.length; i++){
            
            //Possible sum if we include the current element. Then we can add dp[i-2] to this number.
            int a = nums[i] + dp[i-2];

            //If we do not take the current element, we can consider max sum till previous position which is dp[i-1]
            int b = dp[i-1];

            //Take maximum between the above two possibilities
            dp[i] = Math.max(a, b);

        }

        //The final maximum would be at the last position
        return dp[dp.length-1];
    }
}
```
