---
layout: post
title: Missing Number
date: 2022-10-14 12:41 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.
[leetcode](https://leetcode.com/problems/missing-number/)

### Solution

```java
class Solution {
    public int missingNumber(int[] nums) {
        
        int ans = 0;
        
        for(int i=0; i<nums.length; i++) ans ^= nums[i];
        for(int i=1; i<=nums.length; i++) ans ^= i;
        
        return ans;
        
    }
}
```

### Another way to code

```java
class Solution {
    
    public int missingNumber(int[] nums) {
        
        int ans = nums.length;
        
        for(int i=0; i<nums.length; i++) ans ^= nums[i] ^ i;
        
        return ans;
        
    }
    
}
```
