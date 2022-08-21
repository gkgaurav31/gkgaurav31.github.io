---
layout: post
title: Maximum Water between Two Walls"
date: 2022-08-21 16:14 +0530
author: "Gaurav Kumar"
tags: "java arrays two_pointers"
categories: "two_pointers"
---

## Problem Description

Given an array of size N, where A[i] represents the height of the ith wall, pick any two walls such that maximum water can be stored between them.

### Solution

We start from the first and last wall so that we have the largest distance between the walls. If height of water depends on the size of the wall which has smaller value. So, for the next check, we discard the wall which has a smaller value and move to the next wall.

```java
package com.gauk;

import java.util.Arrays;

public class MaxWaterTrapped {

    public static void main(String[] args) {
        int[] A = new int[]{3,7,4,5,2};
        System.out.println(Arrays.toString(new MaxWaterTrapped().getMaxWaterWalls(A)));
    }

    //Given an array of size N, where A[i] represents the height of the ith wall, pick any two walls such that maximum water can be stored between them.
    public int[] getMaxWaterWalls(int A[]){

        int max=0;
        int[] ans = new int[2];
        //Initialize i and j such that the distance between them is maximum
        int i = 0;
        int j = A.length-1;

        while(i<j){

            int distanceBetweenTheWalls = j-i;
            int minWallSize = Math.min(A[i], A[j]);
            int amountOfWater = distanceBetweenTheWalls*minWallSize;

            if(amountOfWater > max){
                max = amountOfWater;
                ans = new int[]{i, j};
            }

            //Check for better values by discarding the wall which is smaller in size
            if(A[i] < A[j]){
                i++;
            }else{
                j--;
            }

        }

        return ans;

    }

}
```
