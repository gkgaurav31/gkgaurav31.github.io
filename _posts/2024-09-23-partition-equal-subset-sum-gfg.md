---
layout: post
title: Partition Equal Subset Sum (geeksforgeeks - SDE Sheet)
date: 2024-09-23 19:15 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[] of size N, check if it can be partitioned into two parts such that the sum of elements in both parts is the same.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/subset-sum-problem2014/1?page=9)

## SOLUTION

### BRUTE-FORCE (TLE)

```java
class Solution{

    static int equalPartition(int n, int arr[])
    {

        int sum = 0;

        for(int i=0; i<n; i++)
            sum += arr[i];

        if(sum%2 == 1)
            return 0;

        int halfSum = sum/2;

        return partitionHelper(n, arr, 0, halfSum, 0) ? 1 : 0;

    }

    static boolean partitionHelper(int n, int[] arr, int currentSum, int halfSum, int idx){

        if(idx == n){
            return currentSum == halfSum;
        }

        // include
        boolean include = partitionHelper(n, arr, currentSum + arr[idx], halfSum, idx+1);

        // exclude
        boolean exclude = partitionHelper(n, arr, currentSum, halfSum, idx+1);

        return include || exclude;
    }

}
```

### DYNAMIC PROGRAMMING (ACCEPTED)

```java
class Solution{

    static int equalPartition(int n, int arr[])
    {
        // Calculate the total sum of the array
        int sum = 0;
        for(int i = 0; i < n; i++)
            sum += arr[i];

        // If the total sum is odd, partitioning into two equal subsets is not possible
        if(sum % 2 == 1)
            return 0;

        // The target sum for each partition will be half of the total sum
        int halfSum = sum / 2;

        // dp[i][j]: if it's possible to form sum j using the first i elements of the array
        boolean[][] dp = new boolean[n][halfSum + 1];

        // It's always possible to form a sum of 0 by using 0 elements
        for(int i = 0; i < n; i++)
            dp[i][0] = true;

        // Loop through the elements
        for(int i = 1; i < n; i++){

            // Is sum j possible using first i elements
            for(int j = 1; j <= halfSum; j++){

                // Option 1: Exclude the current element from the subset
                dp[i][j] = dp[i - 1][j];

                // Option 2: Include the current element in the subset, if it doesn't exceed the current sum
                if (j >= arr[i])

                    // Check if sum j-arr[i] can be formed with previous elements
                    dp[i][j] |= dp[i - 1][j - arr[i]];

            }

        }

        // If dp[n-1][halfSum] is true, it's possible to partition the array into two equal subsets
        return dp[n-1][halfSum] ? 1 : 0;
    }

}
```

### ANOTHER APPROACH

```java
class Solution{

    static int equalPartition(int n, int arr[])
    {
        // Calculate the total sum of the array
        int sum = 0;
        for(int i = 0; i < n; i++)
            sum += arr[i];

        // If the total sum is odd, we cannot split it into two equal subsets
        if(sum % 2 == 1)
            return 0;

        // The target sum for each subset is half of the total sum
        int halfSum = sum / 2;

        // Use a HashSet to store all possible subset sums, starting with 0 (sum of an empty set)
        Set<Integer> sumList = new HashSet<>();
        sumList.add(0);

        // Iterate through each element in the array
        for(int i = 0; i < n; i++){

            // Create a new set to store possible sums by adding the current element
            Set<Integer> currentSumList = new HashSet<>();

            // For each sum already in sumList, add the current element and store the new sum
            for(Integer x : sumList)
                currentSumList.add(arr[i] + x);

            // Add all the new sums to the main set
            sumList.addAll(currentSumList);

        }

        // Check if halfSum is in sumList, meaning we can partition the array into two equal subsets
        return sumList.contains(halfSum) ? 1 : 0;
    }
}
```
