---
layout: post
title: Unique Paths II
date: 2022-11-11 00:22 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.  

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.  

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.  

[leetcode](https://leetcode.com/problems/unique-paths-ii/description/)  

### Solution

```java
class Solution {

    public int uniquePathsWithObstacles(int[][] A) {
               int n = A.length;
       int m = A[0].length;

       int[][] dp = new int[n][m];

       if(A[0][0] == 1) return 0;

       dp[0][0] = 1;

       for(int i=0; i<n; i++){
           if(A[i][0] == 1) break;
           dp[i][0] = 1;
       }

       for(int i=0; i<m; i++){
           if(A[0][i] == 1) break;
           dp[0][i] = 1;
       }

       for(int i=1; i<n; i++){
           for(int j=1; j<m; j++){
               
               if(A[i][j] == 1){
                   continue;
               }

               dp[i][j] = dp[i-1][j] + dp[i][j-1];

           }
       }

       return dp[n-1][m-1];
    }
    
}
```
