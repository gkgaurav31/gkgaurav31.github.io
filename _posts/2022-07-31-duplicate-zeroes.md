---
layout: post
title: Duplicate Zeroes
date: 2022-07-31 23:31 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode important"
categories: "arrays"
---

## Problem Description

Given a fixed-length integer array arr, duplicate each occurrence of zero, shifting the remaining elements to the right.
[Duplicate Zeros](https://leetcode.com/problems/duplicate-zeros/)
Note that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

### Solution

With extra space, this question is quite easy to solve. In-place makes it a bit tough. If we simply move the elements from left to right based on the zeroes we encounter, we will lose the elements on the right. So, obviously we cannot do that.

Let's say we know the count of the zeroes in the array - "c". Now, if we start from the right side, all elements before we meet the first zeroes need to be shifted exactly "c" times. When we encounter the first zero while iterating from right to left, we can say for sure that there are c-1 zeroes on its left. Which means, all the elements on the left side would shift c-1 times. Since the question says that we need to double the zeroes, we do that along the way.

```java
class Solution {
    
    public void duplicateZeros(int[] arr) {
        
        int count = 0; //count of zeroes in the array
        for(int i=0; i<arr.length; i++){
            if(arr[i] == 0) count++;
        }
        
        //The number of time we need to shift some element depends on how many zeroes are on its left. If we encounter a zero element, the elements to its left would shift 1 less than others.
        int shift = count; 
        
        for(int i=arr.length-1; i>=0; i--){
            
            //if the element if 0, decrease the shift
            if(arr[i] == 0) shift --;
            
            //the index to shift will be current index + current shift
            int idx = i+shift;
            
            //if the element is out of bounds, continue
            if(idx > arr.length-1) continue;
            
            //if it's in the range, update the array
            arr[idx] = arr[i];
            
            //since we need to double the zero encountered
            if(arr[i] == 0 && idx+1 < arr.length){
                arr[idx+1] = 0;
            }
            
        }

    }
}
```
