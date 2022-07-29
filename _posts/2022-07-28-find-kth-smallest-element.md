---
layout: post
title: Find Kth Smallest Element
date: 2022-07-28 20:13 +0530
author: "Gaurav Kumar"
tags: "java sorting"
categories: "sorting"
---

## Problem Description

Given N array elements, find Kth smallest element.

>Problem Constraints  
>
> 1 <= |A| <= 100000  
> 1 <= B <= min(|A|, 500)  
> 1 <= A[i] <= 109  

### Solution

Since B can be at max 500, we can use any algorithm to solve the problem.

```java
public class Solution {
    // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
    public int kthsmallest(final int[] A, int B) {

        int arr[] = A.clone();

        for(int i=0; i<B; i++){
            int minIdx = i;
            for(int j=i+1; j<arr.length; j++){
                if(arr[j] < arr[minIdx]){
                    minIdx = j;
                }
            }
            swap(arr, i, minIdx);
        }

        return arr[B-1];

    }

    public void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}
```
