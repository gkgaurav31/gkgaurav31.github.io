---
layout: post
title: Max Min (InterviewBit)
date: 2024-01-10 22:24 +0530
Author: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array A of size N. You need to find the sum of Maximum and Minimum element in the given array.

[interviewbit](https://www.interviewbit.com/problems/max-min-05542f2f-69aa-4253-9cc7-84eb7bf739c4/)

## SOLUTION

We can break the array into two sections and search for the smallest and largest numbers within each part. By performing this process for both halves of the array, we can identify the smallest number in the entire array by comparing the smallest numbers from each half. Similarly, we determine the largest number by comparing the largest numbers from both halves. After finding these smallest and largest values, we add them together and return their sum.

```java
public class Solution {

    public int solve(int[] A) {

        // Find the minimum and maximum elements using minMaxPair function
        int[] minMax = minMaxPair(0, A.length-1, A);

        // Return the sum of the minimum and maximum elements
        return minMax[0] + minMax[1];

    }

    // recursive method to get the [min, max] pair for a given array
    public int[] minMaxPair(int s, int e, int[] A){

        // Calculate the size of the subarray
        int n = e - s + 1;

        // If only one element in the subarray, return it as both minimum and maximum
        if(n == 1) return new int[]{A[s], A[s]};

        // Calculate the midpoint of the subarray (to divide it into two parts)
        int mid = s + (e - s) / 2;

        // Recursively find min and max for left and right halves of the subarray
        int[] left = minMaxPair(s, mid, A);
        int[] right = minMaxPair(mid + 1, e, A);

        // Determine minimum and maximum among the minimums and maximums of left and right halves
        int minArray = left[0] < right[0] ? left[0] : right[0];
        int maxArray = left[1] > right[1] ? left[1] : right[1];

        // Return the minimum and maximum elements of the entire subarray
        return new int[]{minArray, maxArray};
    }
}
```
