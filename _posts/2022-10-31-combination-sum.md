---
layout: post
title: Combination Sum
date: 2022-10-31 20:36 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.  

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.  

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.  

[leetcode](https://leetcode.com/problems/combination-sum/)

### Solution

```java
class Solution {
    
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        
        find(candidates, 0, target, new ArrayList<Integer>());
        return ans;
        
    }
    
    public void find(int[] arr, int i, int target, List<Integer> list){
        
        if(target < 0 || i>=arr.length) return;
        
        if(target == 0){
            ans.add(new ArrayList<>(list));
            return;
        }
            
        //Including
        list.add(arr[i]);
        find(arr, i, target-arr[i], list);

        //Backtrack
        list.remove(list.size()-1); 

        //Excluding
        find(arr, i+1, target, list);
        
    }
}
```
