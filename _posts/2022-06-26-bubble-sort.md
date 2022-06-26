---
layout: post
title: Bubble Sort
date: 2022-06-26 14:10 +0530
author: "Gaurav Kumar"
tags: "java arrays sorting"
categories: "sorting"

---

## Problem Description

Implement Bubble Sort (in-place)

## Solution

We can loop through the array and swap the elements if the right one is lesser than the one towards it's left. We can further optimize it by tracking if we have performed any swaps during a certain iteration. If we did not perform any swap, that indicates that the array is sorted and we can break from the loop. So, in the best case, bubble sort will perform O(n) as the time complexity if we keep a track of swap operations.  

```java
import java.util.*;

class Program {

  public static void swap(int[] array, int i, int j){
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
  
  public static int[] bubbleSort(int[] array) {

    for(int i=0; i<array.length; i++){
      for(int j=i+1; j<array.length; j++){
        if(array[j] < array[i]){
          swap(array, i, j);
        }
      }
    }    

    return array;
  }
}
```
