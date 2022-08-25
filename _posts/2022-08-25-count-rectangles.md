---
layout: post
title: Count Rectangles
date: 2022-08-25 23:37 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing important"
categories: "hashing"
---

## Problem Description

Given two arrays of integers A and B of size N each, where each pair (A[i], B[i]) for 0 <= i < N represents a unique point (x, y) in a 2-D Cartesian plane.

Find and return the number of unordered quadruplet (i, j, k, l) such that (A[i], B[i]), (A[j], B[j]), (A[k], B[k]) and (A[l], B[l]) form a rectangle with the rectangle having all the sides parallel to either x-axis or y-axis.

### Solution

```java
public class Solution {

    public int solve(int[] A, int[] B) {

        int n = A.length;
        Set<String> set = new HashSet<>();

        //Add all co-ordinates to HashSet
        for(int i=0; i<n; i++){
            set.add(A[i] + ":" + B[i]);
        }

        int count = 0;

        //Loop through all the co-ordinates and check if the 2nd co-ordinate forms a diagnol which is not parallel to X or Y axis
        for(int i=0; i<n; i++){

            for(int j=i+1; j<n; j++){

                int x1 = A[i];
                int y1 = B[i];

                int x2 = A[j];
                int y2 = B[j];

                //If the co-ordinates are parallel to X or Y axis, continue.
                if(y1 == y2 || x1 == x2) continue;

                //(x3, y3) and (x4, y4) will be the other two co-ordinates of the rectangle. The order does not matter for us to calculate the count of rectangles.
                int x3 = x2;
                int y3 = y1;

                int x4 = x1;
                int y4 = y2;

                //Check if these two co-ordinates are present in the HashSet. If both are present, then we can form a rectangle
                if(set.contains(x3+":"+y3) && set.contains(x4+":"+y4)) count++;

            }
        }

        //We have incremented the answer twice for every rectangle because every rectangle has two diagonals. So, return count/2.
        return count/2;

    }
}
```
