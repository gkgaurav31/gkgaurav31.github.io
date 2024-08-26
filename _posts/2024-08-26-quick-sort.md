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

The `partition` function is a key part of the Quick Sort algorithm. It rearranges the elements in a section of the array so that all elements less than or equal to a chosen pivot are on its left, and all elements greater than the pivot are on its right. Here's how it works:

1. **Choose a Pivot**: In this example, the pivot is the first element of the current array section (`arr[low]`). We can choose any element.

2. **Initialize Pointers**: Two pointers, `i` (starting from the `low` index) and `j` (starting from the `high` index), are used to scan the array from both ends toward the center.

3. **Move Pointers**: The pointer `i` moves right until it finds an element greater than the pivot. The pointer `j` moves left until it finds an element less than or equal to the pivot.

4. **Swap Elements**: If `i` is still less than `j`, swap the elements at `i` and `j`. This ensures that smaller elements are on the left and larger ones are on the right of the pivot.

5. **Place Pivot**: Once the pointers cross (`i >= j`), swap the pivot element with the element at the `j` pointer. This places the pivot in its correct sorted position.

6. **Return Pivot Position**: The function returns the index `j`, which is now the pivot's final sorted position in the array.

By repeating this process, the array gets sorted around each pivot.

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
