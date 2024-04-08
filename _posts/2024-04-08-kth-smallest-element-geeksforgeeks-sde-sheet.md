---
layout: post
title: Kth smallest element (geeksforgeeks - SDE Sheet)
date: 2024-04-08 21:42 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[] and an integer K where K is smaller than size of array, the task is to find the Kth smallest element in the given array. It is given that all array elements are distinct.

Note :- l and r denotes the starting and ending index of the array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/kth-smallest-element5635/1?page=1)

## SOLUTION

```java
class Solution{
    public static int kthSmallest(int[] arr, int l, int r, int k)
    {

        // max heap
        PriorityQueue<Integer> pq = new PriorityQueue<>( (a,b) -> b-a );

        // loop through all the elements from l to r
        for(int i=l; i<=r; i++){

            // add to max heap
            pq.add(arr[i]);

            // if size of max heap is more than k, remove top most element
            if(pq.size() > k)
                pq.poll();

        }

        // at this point, top of the heap should have kth largest element
        return pq.peek();

    }
}
```
