---
layout: post
title: K-th element of two Arrays (geeksforgeeks - SDE Sheet)
date: 2024-08-25 11:08 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two sorted arrays arr1 and arr2 and an element k. The task is to find the element that would be at the kth position of the combined sorted array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1?page=2)

## SOLUTION

This problem is similar to:

[Median of two sorted array]({% post_url 2022-12-06-median-of-two-sorted-arrays %})

```java
class Solution {

    public long kthElement(int k, int arr1[], int arr2[]) {

        int n = arr1.length;
        int m = arr2.length;

        if(n > m)
            return kthElement(k, arr2, arr1);

        int total = n+m;

        // IMPORTANT:

        // l = Math.max(0, k - m):
        // This represents the smallest possible number of elements from arr1 that can contribute to the first k elements.
        // If m (length of arr2) is large enough that all k elements could potentially come from arr2 alone, l starts at 0.
        // Otherwise, it starts at k - m to ensure at least enough elements are considered from arr1 if not enough can be taken from arr2.

        // r = Math.min(k, n):
        // This represents the largest possible number of elements from arr1 that could be used to contribute to the first k elements.
        // We can't use more than n elements from arr1 since that exceeds the array's size, nor can we use more than k total elements,
        // as we're looking for the k-th element.

        int l = Math.max(0, k - m);
        int r = Math.min(k, n);

        while(l <= r){

            int i = (l + r)/2;
            int j = k - i;

            int l1 = Integer.MIN_VALUE, l2 = Integer.MIN_VALUE;
            int r1 = Integer.MAX_VALUE, r2 = Integer.MAX_VALUE;

            if(i-1 >= 0)
                l1 = arr1[i-1];
            if(i < n)
                r1 = arr1[i];

            if(j-1 >=0)
                l2 = arr2[j-1];

            if(j < m)
                r2 = arr2[j];

            if(l1 <= r2 && l2 <= r1){
                return (long) Math.max(l1, l2);
            }else if(l1 > r2){
                r = i - 1;
            }else
                l = i + 1;


        }

        return -1L;

    }

}
```
