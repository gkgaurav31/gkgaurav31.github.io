---
layout: post
title: Permutations
date: 2022-10-30 21:32 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.  

[leetcode](https://leetcode.com/problems/permutations/)

### Solution

```java
class Solution {
    
    List<List<Integer>> ans = new ArrayList<List<Integer>>();
    
    public List<List<Integer>> permute(int[] nums) {
        find(nums, 0);
        return ans;
    }
    
    public void save(int[] arr){
        List<Integer> list = new ArrayList<>();
        for(int i=0; i<arr.length; i++){
            list.add(arr[i]);
        }
        ans.add(list);
    }
    
    public void find(int[] arr, int i){
        
        if(i == arr.length-1){
            save(arr);
            return;
        }
        
        for(int j=i; j<arr.length; j++){
            swap(arr, i, j);
            find(arr, i+1);
            swap(arr, i, j);
        }
        
    }
    
    public void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
}
```
