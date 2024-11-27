---
layout: post
title: Find the element that appears once in sorted array (geeksforgeeks - SDE Sheet)
date: 2024-08-25 20:15 +0530
author: "Gaurav Kumar"
tags: "java xor geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sorted array arr[] of size N. Find the element that appears only once in the array. All other elements appear exactly twice.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-the-element-that-appears-once-in-sorted-array0624/1?page=2)

## SOLUTION

### USING XOR

```java
class Solution
{
    int findOnce(int arr[], int n)
    {
        int x = arr[0];
        for(int i=1; i<n; i++)
            x = x ^ arr[i];
        return x;
    }
}
```

### USING BINARY SEARCH (OPTIMIZED)

```java
class Solution
{
    int findOnce(int arr[], int n)
    {
        // Initialize pointers for binary search
        int l = 0;
        int r = n - 1;

        while (l <= r) {

            // middle index
            int m = (l + r) / 2;

            int current = arr[m];

            // Check if the current element is equal to the previous element
            if (m - 1 >= 0 && current == arr[m - 1]) {

                // If `m` is odd, all the previous elements must have come up twice.
                // For example: {1, 1, 2, 2, 3, 3, 4, 50, 50, 65, 65}
                // Let's say m = 5
                // If there was any number before this position which came once
                // Then, the position of current would have shifted by 1
                // Since, that is not the case, we should look towards right
                if (m % 2 == 1) {
                    l = m + 1;
                // Otherwise look left
                } else {
                    r = m - 1;
                }
            }
            // In the same way:
            // {1, 1, 2, 2, 3, 3, 4, 50, 50, 65, 65}
            // Let's say m = 4
            // If there was any number before current which was unique,
            // It would have shifted by one position towards left
            else if (m + 1 < n && current == arr[m + 1]) {
                if (m % 2 == 1) {
                    r = m - 1;
                } else {
                    l = m + 1;
                }
            }
            // If the current element is not equal to its neighbors, it is the single element
            else {
                return current;
            }
        }

        // no unique
        return -1;
    }
}
```
