---
layout: post
title: Pick from both sides (Interviewbit)
date: 2024-01-03 21:42 +0530
uthor: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array A of size N.
You have to pick exactly B elements from either left or right end of the array A to get the maximum sum.
Find and return this maximum possible sum.

[interviewbit](https://www.interviewbit.com/problems/pick-from-both-sides/)

## SOLUTION

### APPROACH 1 (Dynamics Programming | Out of Memory)

> Note that this solution was not accepted (failed the hard test due to OOM) so it may not be accurate.

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;

        // i -> elements from left which can be from 0 to n-1
        // j -> elements from right which can be from n-1 to 0
        // so we will need a dp of size n x n
        int[][] dp = new int[n][n];

        //init dp with -1
        for(int i=0; i<n; i++){
            Arrays.fill(dp[i], -1);
        }

        return helper(A, B, 0, A.length-1, dp);
    }

    public int helper(int[] A, int B, int i, int j, int[][] dp){

        // if we have chosen all B elements already, return 0 sum
        if(B == 0) return 0;

        //if the max sum was not calculated for current (i, j) state, calculate & store it
        if(dp[i][j] == -1){

            //max sum, if we select the left element then using recursion for the sub-problem
            int chooseLeft = A[i] + helper(A, B-1, i+1, j, dp);

            //max sum if we select the right element then using recursion for the sub-problem
            int chooseRight = A[j] + helper(A, B-1, i, j-1, dp);

            //take the max from the above choices
            dp[i][j] = Math.max(chooseLeft, chooseRight);

        }

        return dp[i][j];

    }

}
```

### APPROACH 2 (Using Prefix/Suffix Array) - Accepted

```java
public class Solution {

    public int solve(int[] A, int B) {

        int maxSum = Integer.MIN_VALUE;

        int n = A.length;

        //prefix array
        int[] pfl = new int[n];
        pfl[0] = A[0];
        for(int i=1; i<n; i++) pfl[i] = A[i] + pfl[i-1];

        //suffix array
        int[] pfr = new int[n];
        pfr[n-1] = A[n-1];
        for(int i=n-2; i>=0; i--) pfr[i] = A[i] + pfr[i+1];

        for(int i=0; i<=B; i++){

            int l = i;  //choose i elements from the left
            int r = B-i;//choose the rest (B-i) elements from the right

            int leftSum = 0;
            int rightSum = 0;

            if(l > 0) leftSum = pfl[l-1];
            if(r > 0) rightSum = pfr[n-r];

            // update max, if this combination gives a higher sum
            maxSum = Math.max(maxSum, leftSum + rightSum);

        }

        return maxSum;

    }
}
```
