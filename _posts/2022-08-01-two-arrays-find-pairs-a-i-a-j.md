---
layout: post
title: Two Arrays, Find Pairs A[i] > B[j]
date: 2022-08-01 23:50 +0530
author: "Gaurav Kumar"
tags: "java arrays important"
categories: "arrays"
---

## Problem Description

Given two Arrays, find the count of all pairs such that A[i] > B[j]

### Solution

The brute force is to find all pairs such that A[i] > B[j] in the two arrays. To optimize this, we can use sorting. First we sort both the arrays in ascending order. Now we use pointers from left to right, one of each array.

Two things could happen:

- If A[p1] <= B[p2]
This means that the element on first array is smaller. We know that both the arrays are sorted. Since the 2nd array is also sorted, we can say for sure that the current element in A cannot make pairs with any other element in B too, since all other elements will be even larger. So, we simply move the p1 pointer forward to the next element.
- If A[p1] > B[p2]
This means that the current element in array B is SMALLER THAN THE CURRENT ELEMENT IN ARRAY A. Which means, IT WILL BE SMALLER THAN ANY ELEMENT ON THE RIGHT SIDE OF CURRENT ELEMENT OF ARRAY A. So, we can say for sure, that there can be A.length-p1 pairs using this element on array B. Add to the total and move p2 pointer.

```java
public int solve(int[] a, int[] b){

    int count = 0;

    Arrays.sort(a);
    Arrays.sort(b);

    int p1=0, p2=0;

    while(p1<a.length && p2<b.length){
        if(a[p1] <= b[p2]){
            p1++;
        }else{
            count+=(a.length-p1);
            p2++;
        }
    }

    return count;

}
```
