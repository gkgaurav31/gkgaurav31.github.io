---
layout: post
title: Minimum sum partition (geeksforgeeks - SDE Sheet)
date: 2024-10-02 15:51 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr of size n containing non-negative integers, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is minimum and find the minimum difference

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-sum-partition3317/1?page=9)

## SOLUTION

### BRUTE FORCE (TLE)

We can form all subsets using the array elements and track its sum using recursion and backtracking. The sum of second subset will be totalSum - sumOfSubset1. Keep updating the minimum possible value.

```java
class Solution
{
    public int minDifference(int arr[], int n)
    {

        // total sum of the array elements
        int sum = 0;
        for(int i = 0; i < n; i++)
            sum += arr[i];

        minDifferenceHelper(arr, n, sum, new ArrayList<>(), 0, 0);

        return minDiff;
    }

    int minDiff = Integer.MAX_VALUE;

    public void minDifferenceHelper(int[] arr, int n, int totalSum, List<Integer> list, int currentSum, int idx)
    {
        // Base case: If we've processed all elements
        if(idx == n)
        {
            // Calculate the sum of one subset (currentSum) and the other subset (totalSum - currentSum)
            int x = currentSum;
            int y = totalSum - currentSum;

            // Update the minimum difference between the two subset sums
            minDiff = Math.min(minDiff, Math.abs(x - y));
            return;
        }

        // Decision 1: Include the current element in the subset
        list.add(arr[idx]); // Add the element to the subset
        minDifferenceHelper(arr, n, totalSum, list, currentSum + arr[idx], idx + 1); // Recur for the next element

        list.remove(list.size() - 1); // Backtrack to explore other possibilities

        // Decision 2: Exclude the current element from the subset
        minDifferenceHelper(arr, n, totalSum, list, currentSum, idx + 1); // Recur for the next element without adding the current element
    }
}
```

### SMALL OPTIMIZATION (TLE)

We don't really need to maintain the subset1 elements. We can just keep a track of the current sum, if we include the current element at index idx or not.

```java
class Solution
{

    public int minDifference(int arr[], int n)
    {

        int sum = 0;
        for(int i=0; i<n; i++)
            sum += arr[i];

        minDifferenceHelper(arr, n, sum, 0, 0);

        return minDiff;

    }

    int minDiff = Integer.MAX_VALUE;

    public void minDifferenceHelper(int[] arr, int n, int totalSum, int currentSum, int idx){

        if(idx == n){
            int x = currentSum;
            int y = totalSum - currentSum;
            minDiff = Math.min(minDiff, Math.abs(x-y));
            return;
        }

        // take
        minDifferenceHelper(arr, n, totalSum, currentSum + arr[idx], idx + 1);

        // don't take
        minDifferenceHelper(arr, n, totalSum, currentSum, idx+1);

    }

}
```
