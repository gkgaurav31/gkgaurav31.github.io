---
layout: post
title: Pick from both sides!
date: 2023-04-21 21:56 +0530
tags: "java arrays interviewbit"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an integer array A of size N.
You have to pick exactly B elements from either left or right end of the array A to get the maximum sum.
Find and return this maximum possible sum.
NOTE: Suppose B = 4 and array A contains 10 elements then
    You can pick the first four elements or can pick the last four elements or can pick 1 from the front and 3 from the back etc. you need to return the maximum possible sum of elements you can pick.

[interviewbit](https://www.interviewbit.com/problems/pick-from-both-sides/)

### SOLUTION

The brute force is quite straight forward. We calculate the sum from the start and end. We do this by considering that we pick from [0, B] number of elements from the beginning. The remaining elements would be from the end of the array. This works, but we can optimize this by using prefix array.

The logic will be the same, however, to calculate the left and right sum, we can use prefix array.

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;
        
        //prefix array
        int[] pf = new int[n];
        
        pf[0] = A[0];
        for(int i=1; i<n; i++) 
            pf[i] = pf[i-1] + A[i];

        int maxSum = Integer.MIN_VALUE;

        //start by picking 0 to B elements from the beginning
        //that means, we will pick the remaining elements from the end of the array
        for(int takeFromStart=0; takeFromStart<=B; takeFromStart++){

            int current = 0;

            //if we are not taking anything from start, all elements will be from the end
            if(takeFromStart == 0){
                current = rangeSum(n - B, n-1, pf);
            //else, calculate the sum of X elements from the start and B-X elements from the end
            }else{
                current = rangeSum(0, takeFromStart-1, pf) + rangeSum(n - B + takeFromStart, n-1, pf);
                //rangeSum(0, takeFromStart-1, pf) -> we substracted 1 because index starts from 0 in arrays
                //take some examples to understand this better
            }

            //update max
            maxSum = Math.max(maxSum, current);

        }

        return maxSum;

    }

    //Return the sum of elements from L to R using prefix array pf
    public int rangeSum(int L, int R, int[] pf){
        if(L == 0)
            return pf[R];
        else
            return pf[R] - pf[L-1];
    }

}

```
