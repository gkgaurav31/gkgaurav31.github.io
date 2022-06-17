---
layout: post
title: Squares of a Sorted Array
date: 2022-06-16 23:50 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "leetcode"
---

## Problem Description

Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.
[Squares of a Sorted Array](https://leetcode.com/explore/featured/card/fun-with-arrays/521/introduction/3240/)

### Solution

The trivial solution is to calculate power of all the elements and then sort it.

### Intuition

The largest element (the last element in the output array) will either be the square of lowest number (which is on the left) or square of the highest number (which is on the right side) of the original array. So, we can take two pointers - one at the beginning and another at the end. Check which one has a larger power. Add that to the output array (from right to left, because we are first calculating the largest elements). Increment/decrement the value of begin/end pointers as needed.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        
        int[] output = new int[nums.length];
        
        int k=nums.length-1;
        
        for(int i=0, j=nums.length-1; i<=j; ){
            if(square(nums[i]) > square(nums[j])){
                output[k] = square(nums[i]);
                i++;
                k--;
            }else{
                output[k] = square(nums[j]);
                j--;
                k--;
            }
        }
        
        return output;
        
    }
    
    public int square(int n){
        return n*n;
    }
}
```
