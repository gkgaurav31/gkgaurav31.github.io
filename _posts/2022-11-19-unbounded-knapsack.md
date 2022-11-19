---
layout: post
title: Unbounded Knapsack
date: 2022-11-19 22:59 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks dynamic_programming important"
categories: "dynamic_programming"
---

## Problem Description

Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. In other words, given two integer arrays val[0..n-1] and wt[0..n-1] which represent values and weights associated with n items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item or don’t pick it (0-1 property).

[geeksforgeeks](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)  

### Solution

```java
class Solution{
    
    static int knapSack(int N, int W, int val[], int wt[])
    {
        
        int[][] dp = new int[N+1][W+1];
        
        for(int i=0; i<=N; i++){
            Arrays.fill(dp[i], -1);
        }
        
        KP(N, W, val, wt, dp);
        
        return dp[N][W];
        
    }
    
    static int KP(int i, int j, int[] v, int[] w, int[][] dp){
        
        if(i == 0){
            return 0;
        }
        
        if(dp[i][j] == -1){
            
            //leave
            int x = KP(i-1, j, v, w, dp);
            
            //pick
            //Since we can choose the same element again, when picking it up, we don't decrease the ith index.
            if(j>= w[i-1]){
                x = Math.max(x, KP(i, j-w[i-1], v, w, dp) + v[i-1]);
            }
            
            dp[i][j] = x;
            
        }
        
        return dp[i][j];
        
    }
    
}
```
