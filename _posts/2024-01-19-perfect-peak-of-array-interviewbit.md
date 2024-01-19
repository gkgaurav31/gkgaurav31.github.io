---
layout: post
title: Perfect Peak of Array (InterviewBit)
date: 2024-01-19 20:29 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array `A` of size `N`.

You need to check that whether there exist a element which is strictly greater than all the elements on left of it and strictly smaller than all the elements on right of it.

If it exists return `1` else return `0`.

**NOTE:**

Do not consider the corner elements i.e `A[0]` and `A[N-1]` as the answer.

[InterviewBit](https://www.interviewbit.com/problems/perfect-peak-of-array/)

## SOLUTION

We can use a prefix array to maintain the maximum element from left to right. Similarly, we can create a suffix array which has the minimum element from right to left. If the current number at index `i` is more than `prefixLeft[i-1]`, it must be more than all others as well. Similarly, if the current number at index i is less than `suffixright[i+1]`, it must be smaller than all others on its right. So, we have found a number which satisfied the condition and we can return `1`.

```java
public class Solution {

    public int perfectPeak(int[] A) {

        int n = A.length;

        // Creating an array to store the maximum element on the left for each position
        int[] left = new int[n];
        left[0] = A[0];
        for(int i=1; i<n; i++){
            left[i] = Math.max(left[i-1], A[i]);
        }

        // Creating an array to store the minimum element on the right for each position
        int[] right = new int[n];
        right[n-1] = A[n-1];
        for(int i=n-2; i>=0; i--){
            right[i] = Math.min(right[i+1], A[i]);
        }

        // Checking for each position (except the first and last positions)
        for(int i=1; i<n-1; i++){

            int x = A[i];  // Current element at position i

            // If the current element is strictly greater than all elements on its left
            // and strictly smaller than all elements on its right, return 1.
            if(x > left[i-1] && x < right[i+1]){
                return 1;
            }
        }

        // If no such element is found, return 0.
        return 0;

    }

}
```

### ANOTHER WAY TO CODE (SMALL SPACE OPTIMIZATION)

We can only create the prefix array to store the maximum value from left to right.

For the minimum value, we can use a single variable and use carry-forward technique to track the miniumum value while traversing from right to left.

```java
public class Solution {

    public int perfectPeak(int[] A) {

        int n = A.length;

        int[] left = new int[n];

        left[0] = A[0];
        for(int i=1; i<n; i++){
            left[i] = Math.max(left[i-1], A[i]);
        }

        int min = A[n-1];

        for(int i=n-2; i>=1; i--){

            int x = A[i];

            if(x > left[i-1] && x < min){
                return 1;
            }

            min = Math.min(min, x);

        }

        return 0;

    }

}
```
