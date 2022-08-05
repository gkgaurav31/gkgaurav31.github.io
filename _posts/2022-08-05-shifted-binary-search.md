---
layout: post
title: Shifted Binary Search
date: 2022-08-05 00:58 +0530
author: "Gaurav Kumar"
tags: "java arrays sorting important"
categories: "sorting"

---

## Problem Description

Given a sorted array of integers A of size N and an integer B. Array A is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2 ).  

You are given a target value B to search. If found in the array, return its index otherwise, return -1.  
You may assume no duplicate exists in the array.

## Solution

The tricky part in this question is that we do not know the rotation factor, that is, how many times the array has been rotated. If we can somehow find the index of the first element in the second array which will be formed after rotation, we can just apply binary search in the two array, based on the target we are looking for.  

Cases:  

- array[m] < array[m-1] && array[m] > array[m+1]
This is the ideal case in which mid is the indexOfFirstElementOfSecondArray.
- array[m] > array[0]
If the first case is not met, check if the mid element is greater than or less than the first element. If we carefully observe the array, the first element after rotatation will be larger than every element in the second array. It will be smaller than every other element in the first array.  
So, in this case, we land in the first array. Update left = mid + 1
- Otherwise, we have a potential answer. So, save it and reduce the right pointer.  
indexOfFirstElementOfSecondArray = Math.min(indexOfFirstElementOfSecondArray, m);
h = m - 1;

```java
import java.util.*;

class Program {

    public static int binarySearch(int[] array, int target, int start, int end) {

    int left = start;
    int right = end;
    int m = (left + right) / 2;

    while(left <= right){

        if(array[m] > target){
            right = m - 1;
        }
        if(array[m] < target){
            left = m + 1;
        }

        if(array[m] == target){
            return m;
        }

        m = (left + right)/2;
    }

    return -1;

}
  
  public static int shiftedBinarySearch(int[] array, int target) {

    int n = array.length;
    int l=0,h=n-1;
    int indexOfFirstElementOfSecondArray = n-1;

    if(n==1){
      if(array[0] == target) return 0;
      else return -1;
    } 

    if(array[0] == target) return 0;
    if(array[n-1] == target) return n-1;
    l=1;
    h=n-2;
    
    while(l<=h){
      
      int m = (l+h)/2;

      if(array[m] < array[m-1] && array[m] > array[m+1]){
        indexOfFirstElementOfSecondArray = m;
        break;
      }

      if(array[m] > array[0]){
        //landed in first array
        l = m + 1;
      }else{
        indexOfFirstElementOfSecondArray = Math.min(indexOfFirstElementOfSecondArray, m);
        h = m - 1;
      }
      
    }

    int out = -1;
    
    if(target >= array[0]){
      out = binarySearch(array, target, 0, indexOfFirstElementOfSecondArray-1);
    }else{
      out = binarySearch(array, target, indexOfFirstElementOfSecondArray, n-1);
    }
    
    return out;
    
  }
}
```
