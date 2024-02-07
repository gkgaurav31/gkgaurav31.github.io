---
layout: post
title: Search in Bitonic Array! (InterviewBit)
date: 2024-02-07 22:12 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a bitonic sequence `A` of `N` distinct elements, write a program to find a given element B in the bitonic sequence in `O(logN)` time.

NOTE:

A Bitonic Sequence is a sequence of numbers which is first strictly increasing then after a point strictly decreasing.

Problem Constraints

- 3 <= N <= 105
- 1 <= A[i], B <= 108
- Given array always contain a bitonic point.
- Array A always contain distinct elements.

## SOLUTION

```java
public class Solution {

    public int solve(int[] A, int B) {

        // Find the pivot point where the sequence transitions from increasing to decreasing
        int pivot = findPivot(A);

        // Perform binary search twice:
        // First time on the left part (increasing) and then on the second part
        int idx = binarySearchInRange(A, B, 0, pivot, true); // Search in the increasing part
        if(idx == -1)
            idx = binarySearchInRange(A, B, pivot+1, A.length-1, false); // Search in the decreasing part

        return idx;

    }

    // Binary search within a specified range of the sequence
    public int binarySearchInRange(int[] A, int B, int l, int r, boolean isIncreasing){

        while(l <= r){

            int m = (l+r)/2; // midpoint

            if(A[m] == B){ // If the element is found at the midpoint
                return m; // Return its index
            }

            if(isIncreasing){ // If searching in the increasing part

                // If the target is greater than the middle element
                if(B > A[m]){
                    l = m+1; // Look towards the right
                } else {
                    r = m-1; // Otherwise, look towards the left
                }

            } else { // If searching in the decreasing part

                if(B < A[m]){ // If the target is less than the middle element
                    l = m+1; // Look towards the right
                } else {
                    r = m-1; // Look towards the left
                }

            }

        }

        return -1; // If the element is not found, return -1
    }

    public int findPivot(int[] A){

        int n = A.length;

        int l = 0;
        int r = n-1;

        while(l <= r){

            int m = (l+r)/2; // Calculate the midpoint

            if(A[m] > A[m-1] && A[m] < A[m+1]){ // If the middle element is the pivot
                return m; // Return its index

            } else if(A[m] > A[m-1]){ // If the sequence is increasing

                l = m+1; // Adjust the search range to the right half

            } else { // If the sequence is decreasing

                r = m-1; // Adjust the search range to the left half

            }

        }

        return 0; // Default pivot index (shouldn't reach here in a valid bitonic sequence)
    }

}
```
