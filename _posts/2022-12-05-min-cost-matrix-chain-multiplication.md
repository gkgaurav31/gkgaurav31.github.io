---
layout: post
title: Min Cost Matrix Chain Multiplication
date: 2022-12-05 00:22 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks dynamic_programming matrix_chain_multiplication"
categories: "dynamic_programming"
---

### Problem Description

Given a sequence of matrices, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.  

The dimensions of the matrices are given in an array arr[] of size N (such that N = number of matrices + 1) where the ith matrix has the dimensions (arr[i-1] x arr[i]).  

[geeksforgeeks](https://www.geeksforgeeks.org/matrix-chain-multiplication-dp-8/)

### Matrix Chain Multiplication

![snapshot]({{ site.baseurl }}/assets/img/min_cost_matrix_chain_multiplication.png)

```java
public class Solution {
    
    public int solve(int[] A) {

        //number of matrices
        int n = A.length-1; 

        //init
        int[][] dp = new int[n+1][n+1];
        for(int i=0; i<dp.length; i++){
            Arrays.fill(dp[i], -1);
        }

        //final answer will be at dp[1][n]
        return mul(1, n, dp, A);

    }

    public int mul(int i, int j, int[][] dp, int[] v){
        
        //if i and j are same, no need to multiply since it's the matrix itself
        if(i == j) return 0;

        if(dp[i][j] == -1){

            int currentMin = Integer.MAX_VALUE;

            //Example: (A)(BCD), (AB)(CD), (ABC)(D) -> min cost among all options
            for(int k=i; k<j; k++){

                int m1 = mul(i, k, dp, v);
                int m2 = mul(k+1, j, dp, v);

                //additional cost of multiplying the two matrix we got from above
                int c = v[i-1] * v[k] * v[j];

                //update min cost
                currentMin = Math.min(currentMin, m1 + m2 + c);

            }

            dp[i][j] = currentMin;

        }

        return dp[i][j];

    }

}
```
