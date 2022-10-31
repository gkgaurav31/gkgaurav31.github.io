---
layout: post
title: Combination Sum II
date: 2022-10-31 22:54 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.  

Each number in candidates may only be used once in the combination.  

Note: The solution set must not contain duplicate combinations.  

[leetcode](https://leetcode.com/problems/combination-sum-ii/)  

### Solution

The solution is based on [Combination Sum I]({% post_url 2022-10-31-combination-sum %})

```java
class Solution {
    
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        
        Arrays.sort(candidates);
        
        find(candidates, 0, new ArrayList<>(), target);
        return ans;
        
    }
    
    public void find(int[] arr, int i, List<Integer> list, int target){
        
        if(target < 0) return;
        if(i == arr.length){
            if(target == 0){
                ans.add(new ArrayList<Integer>(list));    
            }
            return;
        }
        
        //Including
        list.add(arr[i]);
        find(arr, i+1, list, target-arr[i]);
        
        list.remove(list.size()-1);
        while(i<arr.length-1 && arr[i] == arr[i+1]) i++;
        
        //Excluding
        find(arr, i+1, list, target);
        
    }
}
```
