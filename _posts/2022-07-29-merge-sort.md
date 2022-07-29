---
layout: post
title: Merge Sort
date: 2022-07-29 17:59 +0530
author: "Gaurav Kumar"
tags: "java sorting"
categories: "sorting"
---

## Problem Description

Implement Merge Sort.

### Solution

We will make use of a problem we solved earlier: [Sort a sub-array given s,m,e]({% post_url 2022-07-29-sort-sub-array-in-array-given-s-m-e %}). If we carefully think about any given array, we can divide it into two parts -> Sort them separately -> Then merge both sorted array. For the first half of the array, we can again sort it in the same way. And keep doing it recursively until we have just a single element (which is going to be our base case). Since we have a merge function which takes s, m and e we can use this to merge two sorted array. The important part of the merge function is our assumption. The assumption we take for this recursive function is: __Given A[], sort it from start(s) to end(e)__

```java
package com.gauk;

import java.util.Arrays;

public class Sorting {

    public static void main(String[] args) {

        int[] arr = new int[]{4,8,-1,2,6,3,4,7,13,0};
        mergeSort(arr, 0, arr.length-1);
        System.out.println(Arrays.toString(arr));

    }

    public static void mergeSort(int[] arr, int s, int e){

        if(s==e) return;
        int m = (s+e)/2;
        mergeSort(arr, s, m);
        mergeSort(arr, m+1, e);
        merge(arr, s, m, e);

    }

    //Given an array and index s, m and e, Sort the sub-array from s to e. Consider [s,m] and [m+1,e] is already sorted
    public static void merge(int[] arr, int s, int m, int e){

        int[] temp = new int[e-s+1];

        int i=s;
        int j=m+1;
        int idx=0;

        while(i<=m && j<=e){

            if(arr[i] < arr[j]){
                temp[idx] = arr[i];
                i++;
            }else{
                temp[idx] = arr[j];
                j++;
            }
            idx++;
        }

        for(int x=i; x<=m; x++){
            temp[idx] = arr[x];
            idx++;
        }

        for(int x=j; x<=e; x++){
            temp[idx] = arr[x];
            idx++;
        }

        for(int w=s; w<=e; w++){
            arr[w] = temp[w-s];
        }

    }

}
```
