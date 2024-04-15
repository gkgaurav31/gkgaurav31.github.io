---
layout: post
title: K-th element of two Arrays (geeksforgeeks - SDE Sheet)
date: 2024-04-15 22:16 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two sorted arrays arr1 and arr2 of size N and M respectively and an element K. The task is to find the element that would be at the kth position of the final sorted array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1?page=2)

## SOLUTION

### APPROACH 1 | Time Complexity -> O(n+m)

```java
class Solution {

    // Method to find the kth element in the merged sorted array
    public long kthElement(int arr1[], int arr2[], int n, int m, int k) {

        // Initialize indices for arr1 and arr2
        int i = 0;
        int j = 0;

        // Continue until kth element is found
        while (k > 1) {

            // If all elements of arr1 are already used, move to the next element of arr2
            if (i >= n) {
                j++;
                k--;
                continue;
            }

            // If all elements of arr2 are already used, move to the next element of arr1
            if (j >= m) {
                i++;
                k--;
                continue;
            }

            // Compare elements of arr1 and arr2, move to the smaller one
            if (arr1[i] < arr2[j]) {
                i++;
            } else {
                j++;
            }

            // Decrement k since we've considered one element
            k--;
        }

        // If all elements of arr1 are already used, return the element from arr2
        if (i >= n)
            return arr2[j];

        // If all elements of arr2 are already used, return the element from arr1
        if (j >= m)
            return arr1[i];

        // Return the minimum of current elements from arr1 and arr2
        return Math.min(arr1[i], arr2[j]);
    }

}
```
