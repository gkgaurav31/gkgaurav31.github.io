---
layout: post
title: Delete one
date: 2023-05-07 16:56 +0530
author: "Gaurav Kumar"
tags: "java modulo gcd geeksforgeeks"
categories: "gcd"
---

### PROBLEM DESCRIPTION

Given an integer array A of size N. You have to delete one element such that the GCD(Greatest common divisor) of the remaining array is maximum.
Find the maximum value of GCD.

[geeksforgeeks](https://www.geeksforgeeks.org/remove-an-element-to-maximize-the-gcd-of-the-given-array/)

### SOLUTION

[GCD Theory]({% post_url 2023-05-07-gcd-and-inverse-modulo-theory %})

```java
public class Solution {
    
    public int solve(int[] A) {

        int n = A.length;

        //prefix array from L-R using GCD
        int[] pfl = new int[n];
        //suffix array from R-L using GCD
        int[] pfr = new int[n];

        //init
        pfl[0] = A[0];
        pfr[n-1] = A[n-1];

        for(int i=1; i<n; i++){
            pfl[i] = gcd(pfl[i-1], A[i]);
        }

        for(int i=n-2; i>=0; i--){
            pfr[i] = gcd(pfr[i+1], A[i]);
        }

        //ans
        int max = 1;

        //If we remove element at index i, the GCD for remaining elements will be:
        //GCD of (GCD of elements from [0, i-1], GCD of elements from [i+1, n-1])
        //= GCD of (pfl[i-1], pfr[i+1])
        for(int i=1; i<=n-2; i++){
            max = Math.max(max, gcd(pfl[i-1], pfr[i+1]));
        }

        //If we remove the first element, GCD = pfr[1]
        max = Math.max(max, pfr[1]);

        //If we remove the last element, GCD = pfl[n-2]
        max = Math.max(max, pfl[n-2]);
        
        return max;

    }

    //gcd of a and b
    public int gcd(int a, int b){
        if(b == 0) return a;
        return gcd(b, a%b);
    }

}
```
