---
layout: post
title: Count Inversions
date: 2024-08-24 14:00 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of integers. Find the Inversion Count in the array. Two array elements arr[i] and arr[j] form an inversion if arr[i] > arr[j] and i < j.

Inversion Count: For an array, inversion count indicates how far (or close) the array is from being sorted. If the array is already sorted then the inversion count is 0.
If an array is sorted in the reverse order then the inversion count is the maximum.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1?page=1)

## SOLUTION

[Good Explaination - takeUForward](https://www.youtube.com/watch?v=AseUmwVNaoY)

The merge sort code has been taken from here: [geeksforgeeks](https://www.geeksforgeeks.org/merge-sort/)

Let's say, we want to count such a pair between two arrays arr1 and arr2 and both of them are in their sorted order.

We can start iterating from left to right on both the arrays. Suppose we get an index `i` from `arr1` and index `j` in `arr2` such that `arr1[i] > arr2[j]`, then we can surely say that all elements at index `>= i` in `arr1` will also be larger than `arr2[j]` since the elements are in sorted order. So, we can directly add `size of arr1 - i` to the number of pairs. We can then continue to look at other elements in `arr2`.

This step actually happens during merge sort, while merging two already sorted arrays. We can use this to our advantage. In this merge function, whenever we get into the step where left element is larger than right element, we increase the number of pairs.

```java
class Solution {

    // arr[]: Input Array
    // N : Size of the Array arr[]
    // Function to count inversions in the array.
    static long inversionCount(long arr[], int n) {

        // reset, otherwise it will fail for rest test cases it seems
        count = 0;

        sort(arr, 0, n-1);
        return count;
    }

    static long count = 0;

    // standard merge sort
    static void sort(long arr[], int l, int r)
    {
        if (l < r) {

            // Find the middle point
            int m = l + (r - l) / 2;

            // Sort first and second halves
            sort(arr, l, m);
            sort(arr, m + 1, r);

            // Merge the sorted halves
            merge(arr, l, m, r);
        }
    }


    // standard merge in merge sort
    static void merge(long arr[], int l, int m, int r)
    {
        // Find sizes of two subarrays to be merged
        int n1 = m - l + 1;
        int n2 = r - m;

        // Create temp arrays
        long L[] = new long[n1];
        long R[] = new long[n2];

        // Copy data to temp arrays
        for (int i = 0; i < n1; ++i)
            L[i] = arr[l + i];
        for (int j = 0; j < n2; ++j)
            R[j] = arr[m + 1 + j];

        // Merge the temp arrays

        // Initial indices of first and second subarrays
        int i = 0, j = 0;

        // Initial index of merged subarray array
        int k = l;

        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k] = L[i];
                i++;
            }
            else {

                // THIS IS THE MAIN LOGIC
                // If current left is greater and current right
                // and we know that both the sub arrays are sorted
                // all the elements towards the right on first/left subarray will also be larger
                // so, they will add to the total number of pairs
                count += (n1 - i);

                arr[k] = R[j];
                j++;
            }
            k++;
        }

        // Copy remaining elements of L[] if any
        while (i < n1) {
            arr[k] = L[i];
            i++;
            k++;
        }

        // Copy remaining elements of R[] if any
        while (j < n2) {
            arr[k] = R[j];
            j++;
            k++;
        }

    }


}
```
