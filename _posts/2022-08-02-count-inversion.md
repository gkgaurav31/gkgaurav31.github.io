---
layout: post
title: Count Inversion
date: 2022-08-02 01:17 +0530
author: "Gaurav Kumar"
tags: "java arrays sorting important"
categories: "sorting"
---

## Problem Description

Inversion Count for an array indicates – how far (or close) the array is from being sorted. If the array is already sorted, then the inversion count is 0, but if the array is sorted in the reverse order, the inversion count is the maximum.  
Formally speaking, two elements a[i] and a[j] form an inversion if a[i] > a[j] and i < j
[Count Inversions in an array](https://www.geeksforgeeks.org/counting-inversions/)

### Solution

```java
import java.util.*;

class Program {

  public int countInversions(int[] array) {
    if(array.length <= 1) return 0;
    return solve(array, 0, array.length-1);
  }

  public int solve(int[] a, int s, int e){

      if(s==e) return 0;

      int m = (e+s)/2;
      int l = solve(a, s, m);
      int r = solve(a, m+1,e);

      return l+r+merge(a, s, m, e);

  }

  public int merge(int[] arr, int s, int m, int e){

      int[] temp = new int[e-s+1];

      int i=s;
      int j=m+1;
      int idx=0;
      int count=0;

      while(i<=m && j<=e){

          if(arr[i] <= arr[j]){
              temp[idx] = arr[i];
              i++;
          }else{
              temp[idx] = arr[j];
              j++;
              count+=(m-i+1);
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

      return count;

  }
}
```
