---
layout: post
title: Sort based on Three Numbers
date: 2022-07-03 02:18 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"
---

## Problem Description

You are given an array of 3 integers and another array which contains only the numbers present in the first array. The 3 numbers in the "order" array need not be sorted. You need to return the in-place sorted array according to the first input array. For example: If the order array is [0,1,-1], then the elements in the actual input array must be sorted/arranged in the same order.

### Solution

One way to solve this problem is to use any of the sorting algorithms, however, instead of using >, = or < symbols, we can create our own custom compare method.

```java
import java.util.*;

class Program {

  //return -1 => a < b | 0 => a==b | 1 => a > b
  public int customCompare(int a, int b, int[] order){

    int x = order[0];
    int y = order[1];
    int z = order[2];

    if(a == x){
      if(b == x) return 0;
      if(b == y) return -1;
      if(b == z) return -1;
    }

    if(a == y){
      if(b == x) return 1;
      if(b == y) return 0;
      if(b == z) return -1;
    }

    if(a == z){
      if(b == x) return 1;
      if(b == y) return 1;
      if(b == z) return 0;
    }

    return 0;
    
  }
  
  public int[] threeNumberSort(int[] array, int[] order) {

    for(int i=0; i<array.length; i++){
      for(int j=i+1; j<array.length; j++){
        if(customCompare(array[j],array[i],order) == -1){
          swap(array, i, j);
        }
      }
    }

    return array;
    
  }

  public void swap(int [] array, int i, int j){
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
  
}
```

This can be further optimized in multiple ways:

- Take the first element from the order array. Whenever we get that element in the actual array, swap it to the left part of the array. We would need to maintain a startIndex and increment that by 1 whenever a match is found. Do the same thing for the 3rd element in the order array. However, this time, we will need to keep those elements to the right side of the array and decrement the lastIndex by 1 when a match is found.

- Another naive approach can be to use 3 variables to capture the count of the three elements in the order array. Then, simply use this to update the input array since we know the elements and their count.  

Code for first approach:

```java
import java.util.*;

class Program {
  public int[] threeNumberSort(int[] array, int[] order) {

    int n=array.length;
    int startIndex = 0;

    for(int i=0; i<n; i++){
      if(array[i] == order[0]){
        swap(array, startIndex, i);
        startIndex++;
      }
    }

    int endIndex=n-1;
    
    for(int i=n-1; i>=0; i--){
      if(array[i]==order[2]){
        swap(array, i, endIndex);
        endIndex--;
      }
      
    }

    return array;
    
  }

  public void swap(int[] array, int i, int j){
    int temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}
```
