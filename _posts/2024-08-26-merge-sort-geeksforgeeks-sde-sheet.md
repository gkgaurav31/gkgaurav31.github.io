---
layout: post
title: Merge Sort (geeksforgeeks - SDE Sheet)
date: 2024-08-26 16:30 +0530
author: "Gaurav Kumar"
tags: "java sorting geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[], its starting position l and its ending position r. Sort the array using merge sort algorithm.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/merge-sort/1?page=3)

## SOLUTION

```java
class Solution
{
    void merge(int arr[], int l, int m, int r)
    {

        int[] out = new int[r-l+1];

        int i = l;
        int j = m+1;

        int idx = 0;

        while(i<=m && j<=r){

            if(arr[i] < arr[j]){
                out[idx++] = arr[i++];
            }else{
                out[idx++] = arr[j++];
            }

        }

        while(i <= m)
            out[idx++] = arr[i++];

        while(j <= r)
            out[idx++] = arr[j++];

        for(int k=0; k<out.length; k++){
            arr[k+l] = out[k];
        }

    }

    void mergeSort(int arr[], int l, int r)
    {

        if( l >= r)
            return;

        int m = l + (r-l) / 2;

        mergeSort(arr, l, m);
        mergeSort(arr, m+1, r);

        merge(arr, l, m, r);

    }
}
```
