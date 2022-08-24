---
layout: post
title: Count Right Triangles
date: 2022-08-25 00:56 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing important"
categories: "hashing"
---

## Problem Description

Given two arrays of integers A and B of size N each, where each pair (A[i], B[i]) for 0 <= i < N represents a unique point (x, y) in 2D Cartesian plane.  

Find and return the number of unordered triplets (i, j, k) such that (A[i], B[i]), (A[j], B[j]) and (A[k], B[k]) form a right-angled triangle with the triangle having one side parallel to the x-axis and one side parallel to the y-axis.  

NOTE: The answer may be large so return the answer modulo (109 + 7).

### Solution

```java
public class Solution {

    int m = 1000000007;

    public int solve(int[] A, int[] B) {

        Map<Integer, Integer> setx = new HashMap<>();
        Map<Integer, Integer> sety = new HashMap<>();

        for(int i=0; i<A.length; i++){
            if(setx.containsKey(A[i])){
                setx.put(A[i], setx.get(A[i])+1);
            }else{
                setx.put(A[i], 1);
            }
        }

        for(int i=0; i<B.length; i++){
            if(sety.containsKey(B[i])){
                sety.put(B[i], sety.get(B[i])+1);
            }else{
                sety.put(B[i], 1);
            }
        }

        long total = 0;

        for(int i=0; i<A.length; i++){
            int x = A[i];
            int y = B[i];
            int countX = setx.get(x) - 1;
            int countY = sety.get(y) - 1;
            total = (total%m + (countX%m * countY%m)%m)%m;
        }

        return (int)total%m;

    }
}
```
