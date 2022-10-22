---
layout: post
title: Two Sum II - Input Array Is Sorted
date: 2022-10-22 21:33 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.  

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.  

The tests are generated such that there is exactly one solution. You may not use the same element twice.  

Your solution must use only constant extra space.  

[leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### Solution

### Two Pointer Approach

```java
class Solution {
    
    public int[] twoSum(int[] numbers, int target) {
        
        int i=0;
        int j=numbers.length-1;
        
        while(i<j){
            
            int current = numbers[i] + numbers[j];
            
            if(current == target) return new int[]{i+1, j+1};
            
            if(current > target){
                j--; continue;
            }
            
            if(current < target){
                i++;
                continue;
            }
            
        }
        
        return null;
        
    }
    
}
```
