---
layout: post
title: Min Sum Path in Matrix
date: 2022-11-18 23:26 +0530
tags: "java dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given a M x N grid A of integers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
Return the minimum sum of the path.  
NOTE: You can only move either down or right at any point in time.

### Solution

```java
public class Solution {
    
    public int minPathSum(int[][] A) {

        int n = A.length;
        int m = A[0].length;

        int[][] dp = new int[n][m];

        dp[0][0] = A[0][0];

        for(int i=1; i<n; i++){
            dp[i][0] = A[i][0] + dp[i-1][0];
        }

        for(int i=1; i<m; i++){
            dp[0][i] = A[0][i] + dp[0][i-1];
        }

        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                dp[i][j] = A[i][j] + Math.min(dp[i-1][j], dp[i][j-1]);
            }
        }

        return dp[n-1][m-1];

    }
}
```
