---
layout: post
title: Perfect Squares
date: 2022-11-12 12:54 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

[leetcode](https://leetcode.com/problems/perfect-squares/description/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/perfect_squares.png)

```java
class Solution {

    public int numSquares(int n) {
        
        int[] dp = new int[n+1];
        dp[0] = 0;

        for(int i=1; i<=n; i++){

            int ans=i; //If we take all 1s
            for(int j=1; j*j<=i; j++){
                ans = Math.min(ans, 1 + dp[i-(j*j)]);
            }

            dp[i] = ans;

        }

        return dp[n];

    }
}
```
