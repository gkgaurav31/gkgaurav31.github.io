---
layout: post
title: Contains Duplicate
date: 2022-10-09 13:53 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.  
[leetcode](https://leetcode.com/problems/contains-duplicate/)

### Solution

```java
class Solution {
    
    public boolean containsDuplicate(int[] nums) {
        
        Set<Integer> set = new HashSet<>();
        
        for(int i=0; i<nums.length; i++){
            if(set.contains(nums[i])) return true;
            set.add(nums[i]);
        }
        
        return false;
        
    }
}
```
