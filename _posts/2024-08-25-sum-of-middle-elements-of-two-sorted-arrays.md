---
layout: post
title: Sum of Middle elements of two sorted arrays (geeksforgeeks - SDE Sheet)
date: 2024-08-25 17:43 +0530
author: "Gaurav Kumar"
tags: "java binary_search arrays geeksforgeeks"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given 2 sorted integer arrays `arr1` and `arr2` of the same size. Find the sum of the middle elements of two sorted arrays `arr1` and `arr2`.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/swapping-pairs-make-sum-equal4142/1?page=2)

## SOLUTION

This problem is similar to:

[Median of two sorted array]({% post_url 2022-12-06-median-of-two-sorted-arrays %})

```java
class Solution {

    public int SumofMiddleElements(int[] arr1, int[] arr2) {

        int n = arr1.length;

        int l = 0; // take no elements
        int r = n; // take all elements

        int half = n;  // half of total length, which will be n

        while(l <= r){

            int i = (l + r + 1)/2;

            int j = half - i;

            int l1 = Integer.MIN_VALUE, l2 = Integer.MIN_VALUE;
            int r1 = Integer.MAX_VALUE, r2 = Integer.MAX_VALUE;

            if(i-1 >= 0)
                l1 = arr1[i-1];
            if(i < n)
                r1 = arr1[i];
            if(j-1 >= 0)
                l2 = arr2[j-1];
            if(j < n)
                r2 = arr2[j];

            if(l1 <= r2 && l2 <= r1){
                return Math.max(l1, l2) + Math.min(r1, r2);
            }else if(l1 > r2){
                r = i-1;
            }else
                l = i+1;

        }

        return -1;

    }

}
```
