---
layout: post
title: Sort Sub-Array in Array Given s,m,e
date: 2022-07-29 15:35 +0530
author: "Gaurav Kumar"
tags: "java sorting"
categories: "sorting"
---

## Problem Description

Give an array of N elements and three indices s, m and e in which [s,m] sub-array is sorted and [m+1,e] is also sorted. You need to sort the complete sub-array from [s,e].

### Solution

We can consider the two sub-arrays [s,m] and [m+1,e] as two different arrays which are sorted and then use a two pointer approach to join them. We can use a temporary array of size (e-s+1) which will have the elements from s to e in a sorted way. Finally, simply replace the elements in this temporary array to the original array.

```java
package com.gauk;

import java.util.Arrays;

public class Sorting {

    public static void main(String[] args) {

        int[] arr = new int[]{4,8,-1,2,6,3,4,7,13,0};
        merge(arr, 2, 4, 7);

        System.out.println(Arrays.toString(arr));

    }

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
