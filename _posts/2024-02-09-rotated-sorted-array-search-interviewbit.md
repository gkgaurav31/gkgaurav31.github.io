---
layout: post
title: Rotated Sorted Array Search (InterviewBit)
date: 2024-02-09 22:44 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array of integers `A` of size `N` and an integer `B`.
array `A` is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2 ).

You are given a target value `B` to search. If found in the array, return its index, otherwise, return `-1`.

You may assume no duplicate exists in the array.

NOTE:- Array `A` was sorted in non-decreasing order before rotation.

## SOLUTION

```java
public class Solution {

    public int search(final int[] A, int B) {

        int n = A.length;

        // Initialize left and right pointers for binary search
        int l = 0;
        int r = n - 1;

        while (l <= r) {

            // Calculate mid index
            int m = (l + r) / 2;

            // If the middle element is equal to the target, return its index
            if (A[m] == B)
                return m;

            // Check which part of the array we are currently in

            // Left part: A[m] > A[0]
            if (A[m] > A[0]) {

                // We are on the left part but the target is less than the first element of the array
                // This means, we will need to look on right side
                if (B < A[0]) {
                    l = m + 1;

                // Otherwise, target must be present on the left part itself
                // We choose the direction by comparing A[m] and B
                } else {

                    // If B is lesser, go to left
                    if (B < A[m]) {
                        r = m - 1;
                    // Otherwise go to right
                    } else {
                        l = m + 1;
                    }
                }
            // Right part: A[m] <= A[0]
            } else {

                // We are on the right part but the target would be present in left part as B >= A[0]
                if (B >= A[0]) {
                    r = m - 1;
                } else {

                    // Otherwise, it is present on the right part itself
                    // We decide the direction in the right part by comparing B and A[m]
                    if (B > A[m]) {
                        l = m + 1;
                    } else {
                        r = m - 1;
                    }
                }
            }

        }

        // If target is not found in the array, return -1
        return -1;

    }

}
```
