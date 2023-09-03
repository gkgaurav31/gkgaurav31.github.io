---
layout: post
title: Minimum Path Sum
date: 2023-09-03 12:50 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming leetcode leetcode150"
categories: "dynamic_programming"
---

## PROBLEM DESCRIPTION

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.
Note: You can only move either down or right at any point in time.

[leetcode](https://leetcode.com/problems/minimum-path-sum/)

## SOLUTION

```java
class Solution {

    public int minPathSum(int[][] grid) {

        int n = grid.length; // Number of rows
        int m = grid[0].length; // Number of columns

        // Create a dp array to store the minimum path sum at each cell
        int[][] dp = new int[n][m];

        // Initialize the bottom-right cell of 'dp' with the value from the grid
        // The min path from the last cell with the the last value itself
        dp[n-1][m-1] = grid[n-1][m-1];

        // Iterate through the grid from bottom to top row and right to left column
        for (int r = n - 1; r >= 0; r--) {

            for (int c = m - 1; c >= 0; c--) {

                // If we are at the bottom-right cell, skip the calculation
                if (r == n-1 && c == m-1) continue;

                // Calculate two possible options for the minimum path sum at the current cell:
                int option1 = Integer.MAX_VALUE; // for next move as down
                int option2 = Integer.MAX_VALUE; // for next move as right

                // Check if moving down (if within grid bounds)
                if (r + 1 < n) {
                    option1 = grid[r][c] + dp[r+1][c];
                }

                // Check if moving right (if within grid bounds)
                if (c + 1 < m) {
                    option2 = grid[r][c] + dp[r][c+1];
                }

                // Store the minimum of the two options in 'dp' at the current cell
                dp[r][c] = Math.min(option1, option2);
            }
        }

        // The value at the top-left cell of 'dp' now contains the minimum path sum starting from initial position
        return dp[0][0];
    }
}
```
