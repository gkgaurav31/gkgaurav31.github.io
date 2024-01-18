---
layout: post
title: Move Zeroes (InterviewBit)
date: 2024-01-18 22:37 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array A, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

[InterviewBit](https://www.interviewbit.com/problems/move-zeroes/)

## SOLUTION

### APPROACH 1 - USING EXTRA SPACE

Create another array of size n (`arr`) and initialize `k=0`. Loop through the given array. When we get a non-zero element, put that to `arr[k]` and increment value of k by 1 to go to the next position. Once all the non-zero numbers are put, the remaining position will be anyway be 0 by default. So we can return `arr` as the answer.

```java
public class Solution {

    public int[] solve(int[] A) {

        int n = A.length;

        int[] ans = new int[n];

        int k = 0;

        for(int i=0; i<n; i++){

            if(A[i] != 0){
                ans[k] = A[i];
                k++;
            }

        }

        return ans;
    }

}
```

### APPROACH 2 - CONSTANT SPACE

We can solve this without using any extra space as well. One observation is that the numbers before first 0 does not need any change. So, we can start by getting the index of first 0.

If zero is not found, we can simply return the given array A.

Otherwise, we initialize `idx` to index of first zero. Then, we start iterating from `idx+1`, the index after first zero because those are the positions which may need to change. When we get a non-zero number, swap it with `A[idx]` (which has a 0 currently). So, we have essentially swapped 0 with its nearest non-zero number on the right. This will help in maintaining the order of numbers. Go to the next position where a non-zero number can be put by incrementing `idx++`. At the end of the iterations, all zeroes will be at the end of the array!

```java
public class Solution {

    public int[] solve(int[] A) {

        int n = A.length;

        int idx = -1; // index of the first 0

        // Iterate through the array to find the index of the first 0
        for(int i=0; i<n; i++){
            if(A[i] == 0){
                idx = i;
                break;
            }
        }

        // If no zero is found, the array is already in the desired form, return the array
        if(idx == -1)
            return A;

        // nothing needs to be swapped before the first 0
        // so start from the index after the first 0
        for(int i=idx+1; i<n; i++){

            // If the current element is not 0, swap it with the element at the current index of the first 0
            if(A[i] != 0){
                swap(A, idx, i);
                idx++; // move to next position where a number can be put
            }

        }

        return A;
    }

    // Function to swap elements at positions i and j in the array
    public void swap(int[] A, int i, int j){
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```
