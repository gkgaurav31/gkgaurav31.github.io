---
layout: post
title: Coin Change (Count Ways) (geeksforgeeks - SDE Sheet)
date: 2024-08-25 12:50 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an integer array coins[ ] of size N representing different denominations of currency and an integer sum, find the number of ways you can make sum by using different combinations from coins[ ].  
Note: Assume that you have an infinite supply of each type of coin. And you can use any coin as many times as you want.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/coin-change2448/1?page=2)

## SOLUTION

```java
class Solution {

    // Function to count the number of ways to make a given sum using the provided coins
    public long count(int coins[], int N, int sum) {

        // Initialize a 2D array `dp` to store the results of subproblems
        long[][] dp = new long[N][sum + 1];

        // Fill the `dp` array with -1, indicating that those subproblems haven't been solved yet
        for (int i = 0; i < dp.length; i++)
            Arrays.fill(dp[i], -1);

        // Call the helper function to compute the result
        return countHelper(coins, N, sum, 0, dp);
    }

    // Helper function to count the ways recursively with memoization
    public long countHelper(int coins[], int N, int sum, int idx, long[][] dp) {

        // Base case: If the index exceeds the number of coins, return 0 as no solution exists
        if (idx >= N)
            return 0;

        // Base case: If the sum becomes 0, there's exactly one way to form the sum, which is using no coins
        if (sum == 0)
            return 1;

        // If the subproblem hasn't been solved yet
        if (dp[idx][sum] == -1) {

            long ways = 0;  // Initialize the number of ways to 0

            // Recur for the next coin (excluding the current coin)
            ways += countHelper(coins, N, sum, idx + 1, dp);

            // Recur including the current coin (only if the current coin can contribute to the sum)
            if (sum >= coins[idx])
                ways += countHelper(coins, N, sum - coins[idx], idx, dp);

            // Store the computed number of ways in the `dp` array
            dp[idx][sum] = ways;
        }

        // Return the computed number of ways
        return dp[idx][sum];
    }
}
```
