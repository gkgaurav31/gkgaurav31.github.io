---
layout: post
title: Min Cost Climbing Stairs
date: 2022-11-20 18:56 +0530
author: "Gaurav Kumar"
tags: "java leetcode neetcode150 dynamic_programming"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer array nums, return the length of the longest strictly increasYou are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.  

You can either start from the step with index 0, or the step with index 1.  

Return the minimum cost to reach the top of the floor.  

[leetcode](https://leetcode.com/problems/min-cost-climbing-stairs/description/)  

### Solution

```java
class Solution {

    public int minCostClimbingStairs(int[] cost) {

        int n = cost.length;

        int[] dp = new int[n+1];

        //we can start from 0th or 1st stair
        dp[0] = 0;
        dp[1] = 0;

        for(int i=2; i<=n; i++){
            dp[i] = Math.min(cost[i-1] + dp[i-1], cost[i-2] + dp[i-2]);
        }

        return dp[n];

    }
}
```

### TOP DOWN APPROACH (RECURSION)

```java
class Solution {

    public int minCostClimbingStairs(int[] cost) {
        
        int[] dp = new int[cost.length+1];
        Arrays.fill(dp, -1);

        return climbStairsHelper(cost.length, dp, cost);

    }

    public int climbStairsHelper(int n, int[] dp, int[] cost){

        if(n == 0 || n == 1) return 0;

        if(dp[n] == -1){
            dp[n] = Math.min(cost[n-1] + climbStairsHelper(n-1, dp, cost), cost[n-2] + climbStairsHelper(n-2, dp, cost)); 
        }

        return dp[n];

    }

}
```
