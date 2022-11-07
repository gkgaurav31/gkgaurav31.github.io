---
layout: post
title: Paint House
date: 2022-11-07 23:45 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.  

The cost of painting each house with a certain color is represented by an n x 3 cost matrix costs.  

For example, costs[0][0] is the cost of painting house 0 with the color red; costs[1][2] is the cost of painting house 1 with color green, and so on...  

Return the minimum cost to paint all houses.  

[leetcode](https://leetcode.com/problems/paint-house/description/)  

### Solution

```java
class Solution {
    
    public int minCost(int[][] costs) {
        
        int n = costs.length;

        int[][] dp = new int[n][3];
        dp[0][0] = costs[0][0];
        dp[0][1] = costs[0][1];
        dp[0][2] = costs[0][2];
        
        for(int i=1; i<n; i++){
            dp[i][0] = costs[i][0] + Math.min(dp[i-1][1], dp[i-1][2]);
            dp[i][1] = costs[i][1] + Math.min(dp[i-1][0], dp[i-1][2]);
            dp[i][2] = costs[i][2] + Math.min(dp[i-1][0], dp[i-1][1]);
        }

        return Math.min(dp[n-1][0], Math.min(dp[n-1][1], dp[n-1][2]));

    }
}
```