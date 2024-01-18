---
layout: post
title: Move Zeroes (InterviewBit)
date: 2024-01-18 22:37 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array A, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

[InterviewBit](https://www.interviewbit.com/problems/move-zeroes/)

## SOLUTION

```java
public class Solution {

    public int[] solve(int[] A) {

        int n = A.length;

        int idx = -1; // index of the first 0

        // Iterate through the array to find the index of the first 0
        for(int i=0; i<n; i++){
            if(A[i] == 0){
                idx = i;
                break;
            }
        }

        // If no zero is found, the array is already in the desired form, return the array
        if(idx == -1)
            return A;

        // nothing needs to be swapped before the first 0
        // so start from the index after the first 0
        for(int i=idx+1; i<n; i++){

            // If the current element is not 0, swap it with the element at the current index of the first 0
            if(A[i] != 0){
                swap(A, idx, i);
                idx++; // move to next position where a number can be put
            }

        }

        return A;
    }

    // Function to swap elements at positions i and j in the array
    public void swap(int[] A, int i, int j){
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```
