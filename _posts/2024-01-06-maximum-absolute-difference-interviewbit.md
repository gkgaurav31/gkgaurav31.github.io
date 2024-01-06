---
layout: post
title: Maximum Absolute Difference (InterviewBit)
date: 2024-01-06 23:46 +0530
Author: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given an array of N integers, `A1, A2 ,..., AN`. Return maximum value of `f(i, j)` for all `1 ≤ i, j ≤ N. f(i, j)` is defined as `|A[i] - A[j]| + |i - j|`, where `|x|` denotes absolute value of x.

[interviewbit](https://www.interviewbit.com/problems/maximum-absolute-difference/)

## SOLUTION

We can expand the expression `|A[i] - A[j]| + |i - j|` in the following 4 ways:

> 1. +(A[i] - A[j]) + (i-j) = A[i] - A[j] + i - j = `(A[i] + i) - (A[j] + j)`
> 2. +(A[i] - A[j]) - (i-j) = A[i] - A[j] - i + j = `(A[i] - i) - (A[j] - j)`
> 3. -(A[i] - A[j]) + (i-j) = A[j] - A[i] + i - j = `-(A[i] - i) + (A[j] - j)`
> 4. -(A[i] - A[j]) - (i-j) = A[j] - A[i] + j - i = `-(A[i] + i) + (A[j] + j)`

We can figure out 1st and 4th if we know: `(A[i] + i)` and `(A[j] + j)`-> `arr1`

We can figure out 2nd and 3rd if we know: `(A[i] - i)` and `(A[j] - j)` -> `arr2`

So basically we need two arrays, one with value `A[i] + i` and another with `A[i] - i` for all `i`.

Observe equation 1 and 4: to maximize it, we would need the difference between the max and min present in the array which has `(A[i]+i)` values -> `arr1`

Observe equation 2 and 3: to maximize it, we would need the difference between the max and min present in the array which has `(A[i]-i)` values -> `arr2`

The maximum between these will be our answer.

If we understand the above, the code is quite simple.

```java
public class Solution {

    public int maxArr(int[] A) {

        int n = A.length;

        // Create two arrays, arr1 and arr2, both of size n
        //arr1 -> to store A[i] + i
        //arr2 -> to store A[i] - i
        int[] arr1 = new int[n];
        int[] arr2 = new int[n];

        for(int i=0; i<n; i++){
            arr1[i] = A[i] + i;
            arr2[i] = A[i] - i;
        }

        // Initialize variables to hold minimum and maximum values for arr1 and arr2
        int max1 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE;

        int max2 = Integer.MIN_VALUE;
        int min2 = Integer.MAX_VALUE;

        // Iterate through both arr1 and arr2 to find their maximum and minimum values
        for(int i=0; i<n; i++){

            max1 = Math.max(max1, arr1[i]);
            min1 = Math.min(min1, arr1[i]);

            max2 = Math.max(max2, arr2[i]);
            min2 = Math.min(min2, arr2[i]);

        }

        return Math.max(max1 - min1, max2 - min2);

    }
}
```

## SPACE OPTIMIZATION

We really do not need the `arr1` and `arr2` to store the values of all `A[i] + i` and `A[i] + i`, since we just need the maximum and minimum values from both. So, we can get rid of `arr1` and `arr2`, and find the max/min values during iteration.

```java
public class Solution {

    public int maxArr(int[] A) {

        int n = A.length;

        int max1 = Integer.MIN_VALUE;
        int min1 = Integer.MAX_VALUE;

        int max2 = Integer.MIN_VALUE;
        int min2 = Integer.MAX_VALUE;

        for(int i=0; i<n; i++){

            max1 = Math.max(max1, A[i] + i);
            min1 = Math.min(min1, A[i] + i);

            max2 = Math.max(max2, A[i] - i);
            min2 = Math.min(min2, A[i] - i);

        }

        return Math.max(max1 - min1, max2 - min2);

    }

}
```
