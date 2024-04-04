---
layout: post
title: Find a peak element (InterviewBit)
date: 2024-02-11 17:03 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array of integers `A`, find and return the peak element in it.

An array element is peak if it is NOT smaller than its neighbors.

For corner elements, we need to consider only one neighbor.

For example, for input array `{5, 10, 20, 15}`, `20` is the only peak element.

Following corner cases give better idea about the problem.

1. If input array is sorted in strictly increasing order, the last element is always a peak element.
   For example, `5` is peak element in `{1, 2, 3, 4, 5}`.
2. If input array is sorted in strictly decreasing order, the first element is always a peak element.
   `10` is the peak element in `{10, 9, 8, 7, 6}`.

Note: It is guranteed that the answer is unique.

## SOLUTION

This can be solved using binary search within the range `[0, n-1]` which denotes the index position. If the current mid `m` is not a peak element, then check if we are in increasing or decreasing region based on which we can discard one of the parts.

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;

        int l=0;
        int r=n-1;

        while(l<=r){

            int m = (l+r)/2;

            // for 0th index, just compare with next element on right
            if(m == 0){
                if(A[0] > A[1])
                    return A[0];

                break;
            }

            // for last index, just compare with its previous one
            if(m == n-1){
                if(A[n-1] > A[n-2])
                    return A[n-1];

                break;
            }

            // check if current index is peak element
            if(A[m] >= A[m-1] && A[m] >= A[m+1]){
                return A[m];

            // If current index is not peak
            // Check if it belongs to increasing range
            // Which means, the peak will be on its right
            }else if(A[m] > A[m-1]){
                l = m+1;

            // Otherwise it should be on its left (decreasing)
            }else{
                r = m-1;
            }

        }

        return -1;

    }

}
```
