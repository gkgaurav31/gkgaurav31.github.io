---
layout: post
title: Maximum Sum Value - A[i]*B + A[j]*C + A[k]*D
date: 2022-11-06 18:32 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

You are given an array A of N integers and three integers B, C, and D.  
You have to find the maximum value of ```A[i]*B + A[j]*C + A[k]*D```, where 1 <= i <= j <= k <= N.  

### Solution

If B,C,D were all positive, we could have simply taken the maximum value in the array and multiplied it with all of them. Since they can be negative as well and i<=j<=k need to be true, we need to take a different approach. If we only had a single number

![snapshot]({{ site.baseurl }}/assets/img/max_sum_triplet.png)

```java
public class Solution {

    public int solve(int[] A, int B, int C, int D) {
        
        int n = A.length;

        int dp[][] = new int[3][n];

        dp[0][0] = A[0]*B;
        dp[1][0] = dp[0][0] + A[0]*C;
        dp[2][0] = dp[1][0] + A[0]*D;

        for(int i=1; i<n; i++){
            dp[0][i] = Math.max(A[i]*B, dp[0][i-1]);
            dp[1][i] = Math.max(dp[0][i] + A[i]*C, dp[1][i-1]); 
            dp[2][i] = Math.max(dp[1][i] + A[i]*D, dp[2][i-1]); 
        }

        return dp[2][n-1];
    }
}
```

### ANOTHER WAY TO CODE

```java
public class Solution {
    
    public int solve(int[] A, int B, int C, int D) {
        
        int n = A.length;

        int dp[] = new int[n]; //Instead of using a matrix, we can just use a dp array of size N.

        dp[0] = A[0]*B;
        for(int i=1; i<n; i++){
            dp[i] = Math.max(dp[i-1], A[i]*B);
        }

        dp[0] = dp[0]+A[0]*C;
        for(int i=1; i<n; i++){
            dp[i] = Math.max(dp[i-1], dp[i]+A[i]*C);
        }

        dp[0] = dp[0]+A[0]*D;
        for(int i=1; i<n; i++){
            dp[i] = Math.max(dp[i-1], dp[i]+A[i]*D);
        }

        return dp[n-1];

    }
}
```
