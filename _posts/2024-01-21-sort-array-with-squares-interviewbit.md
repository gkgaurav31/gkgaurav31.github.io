---
layout: post
title: Sort array with squares! (InterviewBit)
date: 2024-01-21 20:35 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a sorted array `A` containing `N` integers both `positive` and `negative`.

You need to create another array containing the squares of all the elements in `A` and return it in `non-decreasing` order.

Try to this in `O(N)` time.

[InterviewBit](https://www.interviewbit.com/problems/sort-array-with-squares/)

## SOLUTION

```java
public class Solution {

    public int[] solve(int[] A) {

        int n = A.length;

        // Find the index of the first non-negative element in the array
        int indexOfFirstNonNegative = getIndexOfFirstNonNegative(A);

        // the first possible answer can be the highest -ve number's square or least +ve number's square, so we start comparing from that position
        int l = indexOfFirstNonNegative - 1;
        int r = indexOfFirstNonNegative;

        // Initialize an array to store the result
        int[] res = new int[n];

        // Initialize an index for the result array
        int idx = 0;

        // while l and r are within the bounds
        while (l >= 0 && r < n) {

            // Calculate the squares of elements at l and r
            int o1 = A[l] * A[l];
            int o2 = A[r] * A[r];

            // Compare the squared values and store in result array accordingly
            if (o1 <= o2) {
                res[idx++] = o1;
                l--; // go to left to get the next possiible value
            } else {
                res[idx++] = o2;
                r++; // go to right to the next possible value
            }

        }

        // it's possible that there are still some remaining elements on the left or right, so we can simply copy them in that order to the result array

        // Copy the remaining elements from the left subarray, if any
        while (l >= 0) {
            res[idx++] = A[l] * A[l];
            l--;
        }

        // Copy the remaining elements from the right subarray, if any
        while (r < n) {
            res[idx++] = A[r] * A[r];
            r++;
        }

        return res;

    }

    // Function to get the index of the first non-negative element in the array
    public int getIndexOfFirstNonNegative(int[] A) {

        // Iterate through the array to find the first non-negative element
        for (int i = 0; i < A.length; i++)
            if (A[i] >= 0)
                return i;

        // If no non-negative element is found, return the length of the array
        return A.length;
    }

}
```

### OPTIMIZATION

To get the index of first non negative number, we can make use of binary search. Overall, the time complexity of the previous code will still remain O(N).

```java
public int getIndexOfFirstNonNegative(int[] A) {

    int n = A.length;

    int l=0;
    int r=n-1;

    int idx = n;

    while(l<=r){

        int m = (l+r)/2;

        // the value at mid is >= 0, so record it
        // however, look for better answer which cannot be on the right of index m
        // so reduce the right to m-1
        if(A[m] >= 0){
            idx = m;
            r = m-1;
        // otherwise, left will have lower values, so look towards the right and update l to m+1
        }else{
            l = m+1;
        }

    }

    return idx;

}
```
