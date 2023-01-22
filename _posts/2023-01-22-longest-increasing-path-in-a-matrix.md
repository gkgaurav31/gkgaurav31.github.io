---
layout: post
title: "Longest Increasing Path in a Matrix"
date: 2023-01-22 01:46 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an m x n integers matrix, return the length of the longest increasing path in matrix.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).  

[leetcode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

### SOLUTION

```java
class Solution {

    public int longestIncreasingPath(int[][] matrix) {
        
        int m = matrix.length;
        int n = matrix[0].length;

        int[][] dp = new int[m][n];

        for(int i=0; i<dp.length; i++) Arrays.fill(dp[i], -1);

        int max = 1;

        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                max = Math.max(max, LIP(matrix, i, j, dp, new boolean[m][n]));
            }
        }

        return max;

    }

    public int LIP(int[][] m, int i, int j, int[][] dp, boolean[][] visited){

        //if out of bounds, return 0
        if(i < 0 || j < 0 || i >= m.length || j >= m[0].length) return 0;

        //if dp is not processes for index i, j
        if(dp[i][j] == -1){

            //direction helper
            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};

            //longest increasing will be at least 1
            int max = 1;

            //check all 4 directions
            for(int k=0; k<4; k++){
                
                //if the next hop is within the limits
                if(i+x[k] >=0 && j+y[k] >= 0 && i+x[k] < m.length && j+y[k] < m[0].length){

                    //if the number at next hop is greater than the current number
                    if(visited[i+x[k]][j+y[k]] == false && m[i+x[k]][j+y[k]] > m[i][j]){

                        //mark as visited
                        visited[i+x[k]][j+y[k]] = true;

                        //update max if it's greater
                        //we add 1 before calling LIP for next hop since we have one element in our increasing sequence
                        max = Math.max(max, 1 + LIP(m, i+x[k], j+y[k], dp, visited));

                        //backtrack
                        visited[i+x[k]][j+y[k]] = false;
                    }
                    
                }
                
            }

            dp[i][j] = max;

        }

        return dp[i][j];

    }
}
```
