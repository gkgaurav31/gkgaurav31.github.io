---
layout: post
title: Wave Array (InterviewBit)
date: 2024-01-14 15:02 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## Problem Description

Given an array of integers A, sort the array into a wave-like array and return it.
In other words, arrange the elements into a sequence such that

`a1 >= a2 <= a3 >= a4 <= a5.....`

NOTE: If multiple answers are possible, return the lexicographically smallest one.

[InterviewBit](https://www.interviewbit.com/problems/wave-array/)

### Solution

The approach is simple – first, the array is sorted in ascending order. Then, adjacent elements are swapped to create a sequence where each element is either greater than or equal to its neighboring element. This ensures the lexicographically smallest wave-like array.

After sorting: `[a, b, c, d]`, `a <= b <= c < = d`

After swapping in pairs: `[b, a, d, c]`, `b >= a <= c >= d`

```java
public class Solution {

    public ArrayList<Integer> wave(ArrayList<Integer> A) {

        int n = A.size();

        // Sort the array in ascending order
        Collections.sort(A);

        // Swap adjacent elements to create a wave-like pattern
        for(int i=0; i<n-1; i+=2){
            swap(A, i, i+1);
        }

        return A;
    }

    // Function to swap elements in the array
    public void swap(ArrayList<Integer> A, int i, int j){
        int temp = A.get(i);
        A.set(i, A.get(j));
        A.set(j, temp);
    }
}
```
