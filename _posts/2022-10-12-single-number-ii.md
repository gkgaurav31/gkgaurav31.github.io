---
layout: post
title: Single Number II
date: 2022-10-12 23:26 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation important"
categories: "bit_manipulation"
---

## Problem Description

Given an integer array nums where every element appears three times except for one, which appears exactly once. Find the single element and return it.

You must implement a solution with a linear runtime complexity and use only constant extra space.
[leetcode](https://leetcode.com/problems/single-number-ii/)

### Solution

```java
class Solution {
    public int singleNumber(int[] nums) {
        
        int[] arr = new int[32];
        
        for(int i=0; i<=31; i++){
            int count = countSetForAtPosition(nums, i);
            System.out.println(count);
            if(count%3 != 0) arr[31-i] = 1;
        }
    
        StringBuffer sb = new StringBuffer();
        for(int i=0; i<arr.length; i++){
            sb.append(arr[i]+"");
        }
        
        long l = Long.parseLong(sb.toString(), 2);
        int number = (int) l;
        
        return number;  
        
    }
    
    public int countSetForAtPosition(int[] nums, int position){
        
        int c=0;
        
        for(int i=0; i<nums.length; i++){
            if(checkSetAtPosition(nums[i], position)) c++;
        }
        
        return c;
    }
    
    public boolean checkSetAtPosition(int n, int i){
        n = n>>i;
        return (n&1)==1;
    }
}
```

### Another way to code

```java
class Solution {
    
    public int singleNumber(int[] nums) {
        
        int ans = 0;
        
        for(int i=0; i<=31; i++){
            int count = countSetAtPosition(nums, i);
            if(count%3 != 0){
              ans = ans|(1<<i); //1<<i is equivalent of 2^i. We are basically setting the bit at ith position
            } 
            
        }
    
        return ans;        
    }
    
    public int countSetAtPosition(int[] nums, int position){
        
        int c=0;
        
        for(int i=0; i<nums.length; i++){
            if(checkSetAtPosition(nums[i], position)) c++;
        }
        
        return c;
        
    }
    
    public boolean checkSetAtPosition(int n, int i){
        n = n>>i;
        return (n&1)==1;
    }
    
}
```
