---
layout: post
title: K Sized Subarray Maximum (geeksforgeeks - SDE Sheet)
date: 2024-08-24 20:12 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array `arr[]` of size `N` and an integer `K`. Find the maximum for each and every contiguous subarray of size `K`.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-of-all-subarrays-of-size-k3101/1?page=1)

## SOLUTION

```java
class Solution
{
    static ArrayList <Integer> max_of_subarrays(int arr[], int n, int k)
    {
        // max heap to store array elements along with their indices.
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);

        // Initialize the heap with the first 'k' elements of the array.
        for(int i = 0; i < k; i++){
            pq.add(new int[]{arr[i], i});
        }

        ArrayList<Integer> res = new ArrayList<>();

        // Add the maximum of the first 'k' elements
        res.add(pq.peek()[0]);

        // Loop over the array from the (k+1)-th element to the end.
        for(int i = k; i < n; i++){

            // Remove elements from the heap that are not within the current sliding window.
            // If the index of the element at the top of the heap is outside the window (i.e., less than or equal to 'i - k'),
            // we remove it since it is no longer relevant.
            while(!pq.isEmpty() && pq.peek()[1] <= i - k){
                pq.poll();
            }

            // Add the current element with its index to the heap.
            pq.add(new int[]{arr[i], i});

            // The maximum element for the current window is at the root of the heap.
            res.add(pq.peek()[0]);
        }

        // Return the list of maximums for all subarrays of size 'k'.
        return res;
    }
}
```
