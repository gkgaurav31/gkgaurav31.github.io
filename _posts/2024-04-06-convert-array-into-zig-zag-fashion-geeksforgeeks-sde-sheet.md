---
layout: post
title: Convert array into Zig-Zag fashion (geeksforgeeks - SDE Sheet)
date: 2024-04-06 17:21 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr of distinct elements of size `N`, the task is to rearrange the elements of the array in a zig-zag fashion so that the converted array should be in the below form:

```plaintext
arr[0] < arr[1]  > arr[2] < arr[3] > arr[4] < . . . . arr[n-2] < arr[n-1] > arr[n].
```

**NOTE:** If your transformation is correct, the output will be 1 else the output will be 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/convert-array-into-zig-zag-fashion1638/1?page=1)

## SOLUTION

```java
class Solution{

    public void zigZag(int a[], int n){

        Arrays.sort(a);

        for(int i=1; i+1<n; i+=2){
            swap(a, i, i+1);
        }

    }

    public void swap(int a[], int i, int j){
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

}
```

## OPTIMIZATION - O(n) SOLUTION

```java
class Solution {

    public static void zigZag(int n, int[] arr) {

        // init: check if current element is less than next element (which should be true for 0th index)
        boolean checkLessThan = true;

        // iterate over the elements
        for(int i=0; i<n-1; i++){

            // we need to check if current element is less than next element
            if(checkLessThan){

                // if current element is not less than next element, then swap both
                if( !(arr[i] < arr[i+1]) )
                    swap(arr, i, i+1);

            // we need to check if current element is more than next element
            }else{

                // if current element is not more than next element, then swap both
                if( !(arr[i] > arr[i+1]) )
                    swap(arr, i, i+1);

            }

            // toggle
            // for next pair, the relation should be opposite
            checkLessThan = !checkLessThan;

        }


    }

    public static void swap(int[] arr, int i, int j){
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

}
```
