---
layout: post
title: All Subsets
date: 2022-10-25 22:02 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking leetcode"
categories: "backtracking"
---

## Problem Description

Given an integer array nums of unique elements, return all possible subsets (the power set).  
The solution set must not contain duplicate subsets. Return the solution in any order.

[leetcode](https://leetcode.com/problems/subsets/)

### Solution

For each element, we can two choices - either to include that element or exclude it from the current subset.

```java
class Solution {
    
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> subsets(int[] nums) {
        helper(nums, new ArrayList<>(), 0);
        return ans;
    }
    
    public void helper(int[] nums, List<Integer> current, int idx){
        
        if(idx == nums.length){
            ans.add(new ArrayList<>(current));
            return;
        }
        
        //Including
        current.add(nums[idx]);
        helper(nums, current, idx+1);
        
        //Excluding
        current.remove(current.size()-1);
        helper(nums, current, idx+1);
        
    }
    
    
}
```
