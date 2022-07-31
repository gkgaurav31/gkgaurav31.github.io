---
layout: post
title: Quick Sort
date: 2022-07-31 14:28 +0530
author: "Gaurav Kumar"
tags: "java sorting"
categories: "sorting"
---

## Problem Description

Implement Quick Sort.

### Solution

The main method to implement quick sort is "rearrangeSubArrayOptimized" here. We take the first element of the sub-array (int s). Then we check all other elements in the array. The ones lesser or equal to this element should be on the left side of this element. The greater ones should be on the right side. With this idea, we can determine the position of the "pivot" element if that sub-array was sorted. Using recursion, we do the same thing on the left side of this identified position and on the right side as well. If there is a single element, the array woudl be already sorted.

```java
package com.gauk;

import java.util.Arrays;

public class QuickSort {

    public static void main(String[] args) {
        int[] a = new int[]{18,8,6,3,11,14,23,20,31,27};
        sort(a, 0, a.length-1);
        System.out.println(Arrays.toString(a));
    }

    public static void sort(int[] arr, int s, int e){

        if(s>=e) return; //single or no element
        //if(e-s+1 <= 1) return; -> this would also work

        int position = rearrangeSubArrayOptimized(arr, s, e);
        sort(arr, s, position-1);
        sort(arr, position+1, e);

    }

    public static int rearrangeSubArrayOptimized(int[] arr, int s, int e){

        int n = arr.length;
        int l=s+1, r=e; //considering the first element of the sub-array as pivot

        while(l<=r){

            if(arr[l] <= arr[s]) l++; //if the element on left is larger than pivot, move forward
            else if(arr[r] > arr[s]) r--;   //if the element on right is larger than pivot, move backward
            else{   //we will reach here only when we have an element on index L which is larger than pivot and another element at index R which is smaller than pivot. Simple, swap these two elements. 
                swap(arr, l, r);
            }

        }

        //To bring the pivot element to its right position, swap it with index R. (We can also swap with L-1)
        swap(arr, s, r);

        return r; //the pivot element's correct position is "r" if the complete array is sorted

    }

    public static void rearrangeOptimized(int[] arr){

        int n = arr.length;
        int l=1, r=n-1;

        while(l<=r){

            if(arr[l] <= arr[0]) l++;
            else if(arr[r] > arr[0]) r--;
            else{
                swap(arr, l, r);
            }

        }

        swap(arr, 0, r);

    }

    public static void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void rearrange(int[] arr){

        if(arr.length == 1) return;

        int[] temp = new int[arr.length];
        int n = arr.length-1;

        int l=0, r=n-1;

        for(int i=1; i<n; i++){
            if(arr[i] <= arr[0]){
                temp[l] = arr[i];
                l++;
            }else{
                temp[r] = arr[i];
                r--;
            }
        }
        temp[l] = arr[0];

        for(int i=0; i<n; i++){
            arr[i] = temp[i];
        }

    }

}
```
