---
layout: post
title: 2 Pair Difference (Two Pointers)
date: 2022-08-21 15:57 +0530
author: "Gaurav Kumar"
tags: "java arrays two_pointers"
categories: "two_pointers"
---

## Problem Description

Given N distinct sorted elements, check if there exists a pair (i,j) such that A[i] - A[j] = k and k>0. (i!=j)

### Solution

Two important thing in question is to figure out where to initilize the two pointers.  

If we keep i=0, j=n-1:  
diff = A[j] - A[i]. Let's say that the diff is smaller than k. If we move i to right, the difference will be smaller. If we move j to the left, we have the same problem. So, we cannot make a decision using this appraoch. To address this problem, we can initialize the i and j to 0 and 1 respectively.

:exclamation: If k is negative, we can simply take mod(k) and do the same thing.

```java
package com.gauk;

public class TwoPairDifference {

    public static void main(String[] args) {
        int[] a = new int[]{-3,0,1,3,6,8,11,14,18,25};
        int[] b = new int[]{1,4,6};

        System.out.println(new TwoPairDifference().check(a, 5));
        System.out.println(new TwoPairDifference().check(b, 2));

    }

    //Given N distinct sorted elements, check if there exists a pair (i,j) such that A[i] - A[j] = k and k>0. (i!=j)
    public boolean check(int[] arr, int k){

        int i=0;
        int j=1;

        while(j<arr.length){

            int diff = arr[j] - arr[i];

            if(diff == k){
                System.out.println(i + " " + j);
                return true;
            }

            else if(diff < k){
                j++;
            }else{
                i++;
            }

            if(i==j){
                j++;
            }

        }

        return false;

    }

}
```
