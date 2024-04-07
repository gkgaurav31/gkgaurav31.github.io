---
layout: post
title: Kadane's Algorithm (geeksforgeeks - SDE Sheet)
date: 2024-04-07 12:32 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array Arr[] of N integers. Find the contiguous sub-array(containing at least one number) which has the maximum sum and return its sum.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/kadanes-algorithm-1587115620/1?page=1)

## SOLUTION

```java
class Solution{

    long maxSubarraySum(int arr[], int n){

        long maxSum = Long.MIN_VALUE;
        long currentSum = 0;

        for(int i=0; i<n; i++){

            currentSum = Math.max(currentSum + arr[i], arr[i]);
            maxSum = Math.max(maxSum, currentSum);

        }

        return maxSum;

    }

}
```
