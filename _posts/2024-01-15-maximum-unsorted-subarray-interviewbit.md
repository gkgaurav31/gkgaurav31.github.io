---
layout: post
title: Maximum Unsorted Subarray (InterviewBit)
date: 2024-01-15 22:41 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## Problem Description

Given an array A of non-negative integers of size N. Find the minimum sub-array `Al, Al+1 ,..., Ar` such that if we sort(in ascending order) that sub-array, then the whole array should get sorted.

If A is already sorted, output `-1`.

[InterviewBit](https://www.interviewbit.com/problems/max-distance/)

### Solution

The idea is actually simple than you'd expect. We create a sorted copy of the original array. If we know the left most element which is not at its correct position and right most element which is also not at its correct position, we can got our subarray that we are looking for! We will definitely need to sort this part of the array to get a completely sorted one.

```java
public class Solution {

    public int[] subUnsort(int[] A) {

        int n = A.length;

        // Create a sorted copy of the array
        int[] sortedA = new int[n];
        for(int i=0; i<n; i++) sortedA[i] = A[i];
        Arrays.sort(sortedA);

        // Initialize pointers for the left and right ends of the unsorted sub-array
        int l = 0;
        int r = n-1;

        // Move the left pointer to the first element that differs in the sorted array
        while(l < n && sortedA[l] == A[l]){
            l++;
        }

        // If the entire array is sorted, return -1
        if(l == n) return new int[]{-1};

        // Move the right pointer to the last element that differs in the sorted array
        while(r >= 0 && sortedA[r] == A[r]){
            r--;
        }

        // Return the indices of the minimum sub-array to be sorted
        return new int[]{l, r};

    }

}
```
