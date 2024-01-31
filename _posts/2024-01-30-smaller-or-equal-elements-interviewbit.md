---
layout: post
title: Smaller or equal elements (InterviewBit)
date: 2024-01-30 20:35 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an sorted array `A` of size `N`. Find number of elements which are less than or equal to `B`.

NOTE: Expected Time Complexity `O(log N)`

## SOLUTION

```java
public class Solution {

    public int solve(int[] A, int B) {

        // Initialize pointers for binary search
        int l = 0; // left pointer
        int r = A.length - 1; // right pointer

        // If the smallest element in the array is greater than B, then there are no elements <= B
        if (A[0] > B) {
            return 0;
        }

        // Initialize index to keep track of the last found element <= B
        int idx = -1;

        // Binary search loop
        while (l <= r) {

            // Calculate the middle index
            int m = (l + r) / 2;

            // If the middle element is greater than B, update the right pointer
            if (A[m] > B) {
                r = m - 1;
            } else {
                // If the middle element is less than or equal to B, update the index and left pointer
                idx = m;
                l = m + 1;
            }

        }

        // Return the count of elements <= B (increment by 1 to get the count)
        return idx + 1;

    }

}
```
