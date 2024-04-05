---
layout: post
title: Equilibrium Point (geeksforgeeks - SDE Sheet)
date: 2024-04-05 23:35 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array `A` of `n` non negative numbers. The task is to find the first equilibrium point in an array. Equilibrium point in an array is an index (or position) such that the sum of all elements before that index is same as sum of elements after it.

Note: Return equilibrium point in 1-based indexing. Return `-1` if no such point exists.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/equilibrium-point-1587115620/1)

## SOLUTION

### APPROACH 1

Using prefix sum array, we can check if left sub-array sub is equal to right sub-array sum.

```java
class Solution {

    public static int equilibriumPoint(long arr[], int n) {

        // Prefix sum array to store cumulative sums of elements in arr[]
        long[] pf = new long[n];

        // Initializing the first element of prefix sum array
        pf[0] = arr[0];

        // Calculating prefix sums for the remaining elements
        for(int i=1; i<n; i++)
            pf[i] = pf[i-1] + arr[i];

        // Iterating through each element to check for equilibrium point
        for(int i=0; i<n; i++){

            // For the first element (i.e., index 0)
            if(i == 0){
                if(rangeSum(pf, 1, n-1) == 0)
                    return i+1; // Returning 1-based index
            }

            // For the last element (i.e., index n-1)
            else if(i == n-1){
                if(rangeSum(pf, 0, n-2) == 0)
                    return i+1; // Returning 1-based index
            }

            // For other elements
            else{

                // Calculate sum of elements to the left of the current element
                long left = rangeSum(pf, 0, i-1);

                // Calculate sum of elements to the right of the current element
                long right = rangeSum(pf, i+1, n-1);

                // If the sums of left and right subarrays are equal,
                // then the current element is an equilibrium point
                if(left == right)
                    return i+1; // Returning 1-based index
            }
        }

        // If no equilibrium point is found, return -1
        return -1;
    }

    // Function to calculate the sum of elements within a specified range
    public static long rangeSum(long[] pf, int l, int r){
        if(l == 0)
            return pf[r];
        else
            return pf[r] - pf[l-1];
    }

}
```

### APPROACH 2

Using a similar logic, but without using extra space. We can take the total sum first `sum`.
Then start looping in from left to right. If we subtract the current element from `sum`, we could get the sum of elements on right. For sum of left, we will keep another variable and use carry forward technique to maintain the sum from left.

```java
class Solution {

    // a: input array
    // n: size of array
    // Function to find equilibrium point in the array.
    public static int equilibriumPoint(long arr[], int n) {

        long rightSum = 0;

        for(int i=0; i<n; i++)
            rightSum += arr[i];

        long leftSum = 0;

        for(int i=0; i<n; i++){

            // to get right sum, we can subtract total sum calculated earlier with the current element
            rightSum = rightSum - arr[i];

            // if left sum equals right sum, return answer
            // for the first iteration, left sum will be 0
            if(leftSum == rightSum)
                return i+1;

            // for next iteration, left sum will change
            // we can simply add the current number to the leftSum to get the "left sub array sum"
            leftSum += arr[i];

        }

        return -1;

    }
}
```
