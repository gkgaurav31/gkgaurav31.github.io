---
layout: post
title: Sub-Array with Given Sum (Two Pointers)
date: 2022-08-21 23:49 +0530
tags: "java arrays two_pointers"
categories: "two_pointers"
---

## Problem Description

Given an array of positive integers A and an integer B, find and return first continuous subarray which adds to B. (First sub-array means the sub-array for which starting index in minimum). Return -1 if such a sub-array does not exist.

### Solution

We can use 2 pointer technique to solve this problem optimally. We can keep pointers left and right. We move right pointer if our sum is < target. We move left pointer when sum > target. To calculate the sum itself, we can make use of prefix sum array.

```java
public class Solution {
    public int[] solve(int[] A, int B) {

        int[] pfx = new int[A.length];

        pfx[0] = A[0];

        for(int i=1; i<A.length; i++){
            pfx[i] = pfx[i-1] + A[i];
        }

        int i=0;
        int j=0;

        while(i<A.length && j<A.length){

            int tempSum=0;

            if(i==0){
                tempSum = pfx[j];
            }else{
                tempSum = pfx[j] - pfx[i-1];
            }

            if(tempSum == B){
                return Arrays.copyOfRange(A, i, j + 1);
            }else if(tempSum < B){
                j++;
            }else{
                i++;
            }

        }

        return new int[]{-1};



    }
}
```
