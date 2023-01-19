---
layout: post
title: Coin Change II
date: 2023-01-19 23:13 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

[leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

### SOLUTION

```java
class Solution {
    
    public int change(int amount, int[] coins) {

        int[][] dp = new int[coins.length][amount+1];

        for(int i=0; i<dp.length; i++) Arrays.fill(dp[i], -1);

        return changeHelper(coins, amount, coins.length-1, dp);

    }

    public int changeHelper(int[] coins, int amount, int idx, int[][] dp){

        if(idx < 0) return 0;

        if(amount == 0) return 1;

        if(dp[idx][amount] == -1){
            
            //skip
            int x = changeHelper(coins, amount, idx-1, dp);

            //pick current coin if possible
            if(amount >= coins[idx]){
                x = x + changeHelper(coins, amount-coins[idx], idx, dp);
            }

            dp[idx][amount] = x;

        }

        return dp[idx][amount];

    }
}
```
