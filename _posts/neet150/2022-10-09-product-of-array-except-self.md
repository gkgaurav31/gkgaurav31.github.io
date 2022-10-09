---
layout: post
title: Product of Array Except Self
date: 2022-10-09 22:56 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].
The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
You must write an algorithm that runs in O(n) time and without using the division operation.  
[leetcode](https://leetcode.com/problems/product-of-array-except-self/)

### Solution

### APPROACH 1

Since we are not supposed to use division, one way to solve this problem is by doing some pre-processing the product of elements on the left side and right side for each index. (Prefix/Suffix Array Approach).
Finally, we simply multiply the ith element in these two array to get the required value.

> Example:
>
> Array:  
> 1   2 3 4
>
> Prefix Product:  
> 1   1 2 6
>
> Suffix Production:  
> 24 12 4 1
>
> Answer:  
> 24 12 8 6

```java
class Solution {
    
    public int[] productExceptSelf(int[] nums) {
        
        int n = nums.length;
        
        int[] lpf = new int[n];
        
        lpf[0] = 1;
        
        for(int i=1; i<n; i++){
            lpf[i] = nums[i-1] * lpf[i-1];
        }
        
        int[] rpf = new int[n];
        rpf[n-1] = 1;
        
        for(int i=n-2; i>=0; i--){
            rpf[i] = rpf[i+1] * nums[i+1];
        }
        
        int[] ans = new int[n];
        
        for(int i=0; i<n; i++){
            ans[i] = lpf[i] * rpf[i];
        }
        
        return ans;
        
    }
}
```
