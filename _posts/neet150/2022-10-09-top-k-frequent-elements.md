---
layout: post
title: Top K Frequent Elements
date: 2022-10-09 15:23 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.  
[leetcode](https://leetcode.com/problems/top-k-frequent-elements/)

### Solution

### APPROACH 1

```java
class Solution {
    
    public int[] topKFrequent(int[] nums, int k) {
        
        Map<Integer, Integer> map = new HashMap<>();
        
        int max = 0;
        
        for(int i=0; i<nums.length; i++){
            
            int x = nums[i];
            
            if(map.containsKey(x)){
                map.put(x, map.get(x) + 1);
            }else{
                map.put(x, 1);
            }
            
            max = Math.max(max, map.get(x));
            
        }
        
        Map<Integer, List<Integer>> m2 = new HashMap<>();
        
        for(Map.Entry<Integer, Integer> entry: map.entrySet()){
            
            int key = entry.getKey();
            int value = entry.getValue();
            
            if(!m2.containsKey(value)){
                List<Integer> l = new ArrayList<>();
                l.add(key);
                m2.put(value, l);
            }else{
                m2.get(value).add(key);
            }
            
        }
        
        
        int[] ans = new int[k];
        int idx=0;
        
        while(k>0){
            if(m2.containsKey(max)){
                List<Integer> temp = m2.get(max);
                for(Integer x: temp){
                    ans[idx] = x;
                    idx++;
                    k--;
                }
            }
            max--;
        }
        
        return ans;
        
    }
    
}
```
