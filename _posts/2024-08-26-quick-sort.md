---
layout: post
title: Quick Sort (geeksforgeeks - SDE Sheet)
date: 2024-08-26 14:38 +0530
author: "Gaurav Kumar"
tags: "java sorting arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Quick Sort is a Divide and Conquer algorithm. It picks an element as a pivot and partitions the given array around the picked pivot.
Given an array arr[], its starting position is low (the index of the array) and its ending position is high(the index of the array).

Note: The low and high are inclusive.

Implement the partition() and quickSort() functions to sort the array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/quick-sort/1?page=3)

## SOLUTION

```java
class Solution
{
    static void quickSort(int arr[], int low, int high)
    {

        // Check if the lower index is less than the higher index.
        // This is the base condition to end the recursion.
        if(low < high) {

            // Partition the array and get the partition index.
            int partitionIndex = partition(arr, low, high);

            // Recursively sort the elements before and after partition index.
            quickSort(arr, low, partitionIndex - 1);  // Sort the left part of the array
            quickSort(arr, partitionIndex + 1, high); // Sort the right part of the array

        }

    }

    // Function to partition the array and return the partition index.
    static int partition(int arr[], int low, int high)
    {
        // Choose the pivot element. Here, the pivot is the first element in the array segment.
        int pivot = arr[low];

        // Initialize two pointers: i starts from the low index and j starts from the high index.
        int i = low;
        int j = high;

        // Repeat the process until the pointers cross each other.
        while(i < j) {

            // Move the 'i' pointer to the right until an element greater than the pivot is found.
            while(i <= high && arr[i] <= pivot)
                i++;

            // Move the 'j' pointer to the left until an element less than or equal to the pivot is found.
            while(j >= low && arr[j] > pivot)
                j--;

            // If the pointers haven't crossed, swap the elements at the 'i' and 'j' pointers.
            if(i < j)
                swap(arr, i, j);

        }

        // The pivot element needs to be placed at its correct position
        // After while loop, j will be at the right most smallest element
        // So, we can safely swap pivot with that position
        swap(arr, low, j);

        // Return the final position of the pivot element.
        return j;

    }

    static void swap(int[] arr, int i, int j){
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```
