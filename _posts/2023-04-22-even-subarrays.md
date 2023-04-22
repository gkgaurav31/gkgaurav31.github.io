---
layout: post
title: Even Subarrays
date: 2023-04-22 22:24 +0530
tags: "java arrays"
categories: "arrays"
---

### PROBLEM DESCRIPTION

You are given an integer array A.
Decide whether it is possible to divide the array into one or more subarrays of even length such that the first and last element of all subarrays will be even.
Return "YES" if it is possible; otherwise, return "NO" (without quotes).

### SOLUTION

```java
public class Solution {

    public String solve(int[] A) {

        int n = A.length;

        //if the array length is odd OR if the first element is odd OR last element is odd, it's not possible to divide
        if(n%2 == 1 || A[0]%2 == 1 || A[n-1]%2 == 1) return "NO";
        else return "YES";

    }

}

```
