---
layout: post
title: Maximum path sum in matrix (geeksforgeeks - SDE Sheet)
date: 2024-08-30 17:45 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a n x n matrix of positive integers. There are only three possible moves from a cell mat[r][c].

- mat[r+1] [c]
- mat[r+1] [c-1]
- mat [r+1] [c+1]

Starting from any column in row 0 return the largest sum of any of the paths up to row n -1. Return the highest maximum path sum.

Note : We can start from any column in zeroth row and can end at any column in (n-1)th row.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/path-in-matrix3805/1?page=4)

## SOLUTION

```java
class Solution {
    static int maximumPath(int n, int mat[][]) {

        // Create a DP array to store the maximum path sums up to each cell
        int[][] dp = new int[n][n];

        // Copy the first row from the input matrix to the DP array
        // This is because we can start from any column in the first row
        // And the max till first row will be first row itself
        for (int c = 0; c < n; c++) {
            dp[0][c] = mat[0][c];
        }

        // Start filling the DP array from the second row onwards
        for (int i = 1; i < n; i++) {

            for (int j = 0; j < n; j++) {

                // init
                int left = 0, top = 0, right = 0;

                // If moving from the top-left diagonal is within bounds
                if (j - 1 >= 0) {
                    left = dp[i-1][j-1] + mat[i][j];
                }

                // Moving straight down from the cell above
                top = dp[i-1][j] + mat[i][j];

                // If moving from the top-right diagonal is within bounds
                if (j + 1 < n) {
                    right = dp[i-1][j+1] + mat[i][j];
                }

                // max of three possible moves
                dp[i][j] = Math.max(left, Math.max(right, top));
            }
        }

        // One of the sums in the last column will be the max, which is our result
        int max = 0;
        for (int c = 0; c < n; c++) {
            max = Math.max(max, dp[n-1][c]);
        }

        return max;
    }
}
```
