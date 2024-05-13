---
layout: post
title: Minimum Swaps to Sort (geeksforgeeks - SDE Sheet)
date: 2024-05-13 22:48 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of n distinct elements. Find the minimum number of swaps required to sort the array in strictly increasing order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-swaps/1?page=2)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=-2_c4lG7k_M)

```java
class Solution {

    public int minSwaps(int nums[]) {

        int n = nums.length;

        // 2D array to store the elements of nums along with their original indices
        int[][] arr = new int[n][2];
        for(int i=0; i<n; i++) {
            arr[i] = new int[]{nums[i], i};
        }

        // Sorting the arr array based on the first element of each subarray (the actual values)
        // This is how the array will look like, if it was sorted
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);

        // Variable to count the number of swaps
        int swaps = 0;

        // Index to traverse the sorted array
        int idx = 0;

        // Loop until we reach the end of the array
        // For each element, we will try to place them to its original position in the array
        // We are going in a reverse way. We sorted the array, then we can trying to put them back in the position they were originally given in
        // Since we had added the indices in the beginning, so we know where it should be placed. So, we will swap to that position
        while(idx < n) {

            // If the current element is at its correct position, move to the next element
            if(arr[idx][1] == idx) {
                idx++;
            } else {
                // If not, swap the current element with the element at its correct position
                // After putting the current element to its correct position, it is possible that the new element at idx position is still incorrect
                // So, we don't increment idx, and check the new element again
                swap(arr, idx, arr[idx][1]);

                // Increment the swap count
                swaps++;
            }
        }

        return swaps;
    }

    // Function to swap two elements in the array
    public void swap(int[][] arr, int i, int j) {
        int[] t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```
