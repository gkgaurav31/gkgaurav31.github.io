---
layout: post
title: Unique Paths
date: 2022-11-07 23:21 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## Problem Description

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.  

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.  

[leetcode](https://leetcode.com/problems/unique-paths/description/)  

### Solution

```java
class Solution {
    public int uniquePaths(int m, int n) {

        int[][] dp = new int[n][m];

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(i == 0 || j == 0){
                    dp[i][j] = 1; continue;
                }
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[n-1][m-1];
    }
}
```
