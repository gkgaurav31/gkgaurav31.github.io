---
layout: post
title: Max Sum Increasing Subsequence (geeksforgeeks - SDE Sheet)
date: 2024-09-25 00:20 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of n positive integers. Find the sum of the maximum sum subsequence of the given array such that the integers in the subsequence are sorted in strictly increasing order i.e. a strictly increasing subsequence.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-sum-increasing-subsequence4749/1?page=9)

## SOLUTION

```java
class Solution
{
    // Method to find the maximum sum of an increasing subsequence
    public int maxSumIS(int arr[], int n)
    {

        int maxSum = 0;

        // dp[i] represents the maximum sum of an increasing subsequence ending with arr[i]
        int[] dp = new int[n];

        // dp[i] will be at least equal to that element itself
        for(int i = 0; i < n; i++)
            dp[i] = arr[i];

        // Initialize maxSum with the first element of dp
        maxSum = dp[0];

        // Loop over the array to find the maximum sum increasing subsequence
        for(int i = 1; i < n; i++){

            // Check all previous elements to find subsequences that can be extended by arr[i]
            for(int j = 0; j < i; j++){

                // If arr[i] is greater than arr[j], we can extend the subsequence ending at j
                if(arr[i] > arr[j]){

                    // Update dp[i] to reflect the maximum possible sum ending at arr[i]
                    dp[i] = Math.max(dp[i], dp[j] + arr[i]);

                }
            }

            // Update maxSum with the maximum value found so far
            maxSum = Math.max(maxSum, dp[i]);
        }

        return maxSum;
    }
}
```
