---
layout: post
title: Count number of hops (geeksforgeeks - SDE Sheet)
date: 2024-09-21 23:58 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

A frog jumps either 1, 2, or 3 steps to go to the top. In how many ways can it reach the top of Nth step. As the answer will be large find the answer modulo 1000000007.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/count-number-of-hops-1587115620/1?page=8)

## SOLUTION

We can solve this using dynamic programming by counting how many ways the frog can reach each step based on the previous three steps. To reach step i, the frog can come from step i-1, i-2, or i-3, so the total ways to reach step i is the sum of the ways to reach those three steps.

```java
class Solution {

    // Define the modulo value as a constant, since the answer might be large.
    private static final long M = 1000000007;

    static long countWays(int n) {

        if(n == 0 || n == 1)
            return 1;

        if(n == 2)
            return 2;

        if(n == 3)
            return 4;

        // Create an array to store the number of ways to reach each step.
        long[] dp = new long[n+1];

        // Initialize base cases.
        dp[0] = 1; // 1 way to stay on the ground (0 steps).
        dp[1] = 1; // 1 way to reach step 1 (1 step).
        dp[2] = 2; // 2 ways to reach step 2 (1+1 or 2).
        dp[3] = 4; // 4 ways to reach step 3 (1+1+1, 1+2, 2+1, or 3).

        // Fill the dp array for steps greater than 3.
        // The number of ways to reach step i is the sum of the ways to reach the previous
        // three steps: dp[i-1], dp[i-2], and dp[i-3], taken modulo M.
        for(int i = 4; i <= n; i++){
            dp[i] = (dp[i-1] % M + dp[i-2] % M + dp[i-3] % M) % M;
        }

        return dp[n];

    }

}
```
