---
layout: post
title: Maximum sum such that no two elements are adjacent
date: 2022-11-12 13:30 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given an array arr[] of positive numbers, The task is to find the maximum sum of a subsequence such that no 2 numbers in the sequence should be adjacent in the array.

[geeksforgeeks](https://www.geeksforgeeks.org/maximum-sum-such-that-no-two-elements-are-adjacent/)  

### Solution

```java
class Solution
{

    public int FindMaxSum(int arr[], int n)
    {
        
        //Edge cases
        if(n == 0) return 0;
        if(n == 1) return arr[0];
        
        int[] dp = new int[n];
        
        //Single element, max is itself
        dp[0] = arr[0];

        //If N=2, max is the max out of those two elements since we cannot select both as they are adjacent
        dp[1] = Math.max(arr[0], arr[1]);
        
        //Set current max
        int maxSum = dp[1];
        
        //Include other numbers one by one and check for max
        for(int i=2; i<n; i++){
            
            //select current
            //So, we cannot select previous number
            //We can add max that we found for elements from index 0 to i-2 -> which is dp[i-2]
            int option1 = arr[i] + dp[i-2];
            
            //don't select current
            //So the max will be the max for elements from index 0 to i-1 -> which is dp[i-1]
            int option2 = dp[i-1];
            
            dp[i] = Math.max(option1, option2);

            maxSum = Math.max(maxSum, dp[i]);
            
        }
        
        return maxSum;
        
    }
}
```
