---
layout: post
title: Segregate 0s and 1s in an array (InterviewBit)
date: 2024-01-19 22:51 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given an array of 0s and 1s in random order. Segregate 0s on left side and 1s on right side of the array [Basically you have to sort the array]. Traverse array only once.

[InterviewBit](https://www.interviewbit.com/problems/segregate-0s-and-1s-in-an-array/)

## SOLUTION

This is a pretty simple one. First find the count of zeroes. Then, fill the complete array with 1. The first half needs to be zeroes. So, loop from left and start adding the zeroes based on the count calculated earlier.

```java
public class Solution {

    public int[] solve(int[] A) {

        int n = A.length;

        int count0 = 0;

        for(int i=0; i<n; i++){
            if(A[i] == 0) count0++;
        }

        Arrays.fill(A, 1);

        for(int i=0; i<count0; i++){
            A[i] = 0;
        }

        return A;

    }

}
```
