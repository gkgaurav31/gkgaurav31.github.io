---
layout: post
title: Minimize the sum of product (geeksforgeeks - SDE Sheet)
date: 2024-04-06 17:32 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given two arrays, `A` and `B`, of equal size `N`. The task is to find the minimum value of `A[0] * B[0] + A[1] * B[1] + .... + A[N-1] * B[N-1]`, where shuffling of elements of arrays `A` and `B` is allowed.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimize-the-sum-of-product1525/1?page=1)

## SOLUTION

```java
class Solution {

    public long minValue(long a[], long b[], long n)
    {

        Arrays.sort(a);
        Arrays.sort(b);

        long res = 0;

        for(int i=0; i<n; i++){

            long x = a[i];
            long y = b[(int)(n - i - 1)];

            res += (x*y);

        }

        return res;

    }

}
```
