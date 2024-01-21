---
layout: post
title: Maximum Consecutive Gap (InterviewBit)
date: 2024-01-21 14:09 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an unsorted array, find the maximum difference between the successive elements in its sorted form.
Return 0 if the array contains less than 2 elements.

-You may assume that all the elements in the array are non-negative integers and fit in the 32-bit signed integer range.

- You may also assume that the difference will not overflow.

Try to solve it in linear time/space

[InterviewBit](https://www.interviewbit.com/problems/maximum-consecutive-gap/)

## SOLUTION

### APPROACH 1 - SORTING

```java
public class Solution {

    public int maximumGap(final int[] A) {

        int n = A.length;

        if(n < 2) return 0;

        Arrays.sort(A);
        int maxDiff = 0;

        for(int i=1; i<n; i++){
            maxDiff = Math.max(maxDiff, A[i] - A[i-1]);
        }

        return maxDiff;

    }

}
```

### APPROACH 2 -
