---
layout: post
title: 2 Pair Sum (Two Pointers)
date: 2022-08-21 15:31 +0530
author: "Gaurav Kumar"
tags: "java arrays two_pointers"
categories: "two_pointers"
---

## Problem Description

Given N distinct sorted elements, check if there exists a pair (i,j) such that A[i] + A[j] = k, i!=j.  

### Solution

The brute force approch is to check all pairs - O(N^2)  
A better way is to use hashmap. Keep adding numbers in the HashMap and while iterating also check if (k-A[i]) is present. If it's present, return true. This will be O(N) for both time and space.  
We can also use binary search to check for the 2nd number (k-A[i]) in rest of the array. This will be O(NlogN) and constat space.  
Using the two pointer approach, we can solve this problem in O(N) time and O(1) space comlexity, taking advantage of the fact that the array is sorted.

```java
package com.gauk;

public class TwoPairSum {

    public static void main(String[] args) {
        System.out.println(new TwoPairSum().check(new int[]{-3,0,1,3,6,8,4,14,18,25}, 15));
        System.out.println(new TwoPairSum().check(new int[]{-3,0,1,3,6,8,4,14,18,25}, 99));
    }

    //given N distinct sorted elements, check if there exists a pair (i,j) such that A[i] + A[j] = k, i!=j.
    public boolean check(int[] arr, int k){

        int i=0;
        int j=arr.length-1;

        while(i<j){

            int currentSum = arr[i] + arr[j];

            if(currentSum == k) return true;

            else if(currentSum > k){
                j--;
            }else{
                i++;
            }
        }

        return false;

    }

}
```
