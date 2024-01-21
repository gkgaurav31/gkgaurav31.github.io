---
layout: post
title: Balance Array (InterviewBit)
date: 2024-01-21 12:14 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array `A` of size `N`. You need to count the number of special elements in the given array.

A element is special if removal of that element make the array balanced.

Array will be balanced if sum of even index element equal to sum of odd index element.

[InterviewBit](https://www.interviewbit.com/problems/triplets-with-sum-between-given-range/)

## SOLUTION

The main part to understand in this question is that if we remove an element at index i, the even/odd sum on its left will remain the same, however, on the right side - even sum will change to odd and vice versa. Because each index on the right will move by 1 position (so even becomes odd and odd becomes even).

We can get the even/odd sum using prefix sum array.

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;

        // Calculate prefix sums for even and odd indices separately
        int[] evenPf = getEvenPrefixArray(A);
        int[] oddPf = getOddPrefixArray(A);

        // Counter to track the number of special elements
        int count = 0;

        // For each position, check the even and odd sum if ith element is removed
        for(int i=0; i<n; i++){

            int evenSum = 0;
            int oddSum = 0;

            // Calculate evenSum and oddSum based on the index i

            // if the first element is removed
            if(i == 0){
                evenSum = getRangeSum(oddPf, i+1, n-1);
                oddSum = getRangeSum(evenPf, i+1, n-1);

            // if the last element is removed
            }else if(i == n-1){
                evenSum = getRangeSum(evenPf, 0, i-1);
                oddSum = getRangeSum(oddPf, 0, i-1);

            // if any of the middle elements is removed
            }else{
                evenSum = getRangeSum(evenPf, 0, i-1) + getRangeSum(oddPf, i+1, n-1);
                oddSum = getRangeSum(oddPf, 0, i-1) + getRangeSum(evenPf, i+1, n-1);
            }

            // Check if removing the current element makes the array balanced
            if(evenSum == oddSum){
                count++;
            }
        }

        return count;
    }

    // sum of elements in the given range [s, e] of the prefix array
    public int getRangeSum(int[] pf, int s, int e){
        if(s == 0){
            return pf[e];
        }else{
            return pf[e] - pf[s-1];
        }
    }

    // prefix sum for even indices
    public int[] getEvenPrefixArray(int[] A){
        int n = A.length;
        int[] pf = new int[n];
        pf[0] = A[0];

        for(int i=1; i<n; i++){
            if(i%2 == 1){
                pf[i] = pf[i-1];
            }else{
                pf[i]  = pf[i-1] + A[i];
            }
        }

        return pf;
    }

    // prefix sum for odd indices
    public int[] getOddPrefixArray(int[] A){
        int n = A.length;
        int[] pf = new int[n];
        pf[0] = 0; // Starting from 0 for odd indices

        for(int i=1; i<n; i++){
            if(i%2 == 0){
                pf[i] = pf[i-1];
            }else{
                pf[i]  = pf[i-1] + A[i];
            }
        }

        return pf;
    }
}
```
