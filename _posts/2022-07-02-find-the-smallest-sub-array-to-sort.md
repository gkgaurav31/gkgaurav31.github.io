---
layout: post
title: Find the Smallest Sub-Array to Sort
date: 2022-07-02 20:21 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"
---

## Problem Description

You are given an array which could be unsorted. Return the start and end index of the smallest sub-array which if-sorted will make the complete array sorted.

### Solution

Think about how you can tell if any element is at its correct position. Since we know that the final array is sorted, we can say this:
If all the elements on its right side are greater than or equal to the current element & if all the elements on the left side are smaller or equal to the current element, then that element is at its correct position. Using this approach, we can determine the first and last index which violates it. To optimize it further, we can create a prefix/suffix array containing the max/min values for each index. For example, we can create pfMin to store the min value till the current index (from right side). Similar, create pfMax to store the max value till the current element (from left side). Using this pfMin/pfMax, we can dedice if the current index is in its correct position.

![snapshot]({{ site.baseurl }}/assets/img/sort_sub_array.jpg)

```java
import java.util.*;

class Program {
  public static int[] subarraySort(int[] array) {

    int n = array.length;

    int[] pfMin = new int[n];
    int[] pfMax = new int[n];

    int min = Integer.MAX_VALUE;
    pfMin[n-1] = array[n-1];
    for(int i=n-2; i>=0 ;i--){
      
      if(array[i] < min){
        min = array[i];
      }
      pfMin[i] = Math.min(min, pfMin[i+1]);

    }

    int max = Integer.MIN_VALUE;
    pfMax[0] = array[0];
    for(int i=1; i<n; i++){

      if(array[i] > max){
        max = array[i];
      }
      
      pfMax[i] = Math.max(max, pfMax[i-1]);

    }

    int start=-1, end=-1;

    for(int i=0; i<n-1; i++){
      if(pfMin[i+1] < array[i]){
        start = i; break;
      }
    }

    for(int i=n-1; i>=1; i--){
      if(pfMax[i-1] > array[i]){
        end = i; break;
      }
    }

    return new int[]{start, end};
    
  }
}
```
