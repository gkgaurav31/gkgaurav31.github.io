---
layout: post
title: Best Time to Buy and Sell Stock IV
date: 2023-09-16 19:49 +0530
author: "Gaurav Kumar"
tags: "java leetcode leetcode150 recursion dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.
Find the maximum profit you can achieve. You may complete at most k transactions: i.e. you may buy at most k times and sell at most k times.
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

[leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

## Solution

This problem can be exactly solved like this question: [Singleton]({% post_url 2023-07-05-design-pattern-singleton %})
The cap value states can be from: [0, 1, 2, ..., k] -> k+1 states. This is the only change needed.

```java
class Solution {
    public int maxProfit(int k, int[] prices) {

        int[][][] dp = new int[prices.length][2][k+1];

        for(int i=0; i<dp.length; i++){
            for(int j=0; j<dp[0].length; j++){
                Arrays.fill(dp[i][j], -1);
            }
        }

        return helper(prices, 0, 1, k, dp);
    }


    public int helper(int[] prices, int idx, int buy, int cap, int[][][] dp){

        //if don't have any stock left or if the number of transactions left is 0, return 0
        if(idx == prices.length || cap == 0)
            return 0;

        //if the value of current state: [index, buy/sell, cap] has not been computed previously
        if(dp[idx][buy][cap] == -1){

            //if we can buy a stock
            if(buy == 1){

                //we have two options:
                //1. buy the current stock and move forward. We reduce the profit by prices[idx] since we will be investing that much.
                //2. skip buying, so 0 profit added in this step.
                //we will take the maximum profit possible between these two options
                dp[idx][buy][cap] = Math.max(
                    -prices[idx] + helper(prices, idx+1, 0, cap, dp),
                    0 + helper(prices, idx+1, 1, cap, dp)
                );

            }else{ //if we cannot buy the current stock (allowed to sell)

                //we have two options:
                //1. we can sell it with a profit of prices[i]
                //2. Skip the current stock and continue to be in same state (not buy)
                dp[idx][buy][cap] = Math.max(
                    prices[idx] + helper(prices, idx+1, 1, cap-1, dp),
                    0 + helper(prices, idx+1, 0, cap, dp)
                );
            }


        }

        return dp[idx][buy][cap];

    }

}
```
