---
layout: post
title: Group Anagrams
date: 2022-10-09 14:27 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an array of strings strs, group the anagrams together. You can return the answer in any order.
An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.  
[leetcode](https://leetcode.com/problems/contains-duplicate/)

### Solution

The idea is that the sorted version of two strings which are Anagrams of each other will be same. So, we key this sorted form as the Key of a HashMap and keep insert the strings in the list of string (value for that key).  

```java
class Solution {
    
    public List<List<String>> groupAnagrams(String[] strs) {
        
        Map<String, List> map = new HashMap<>();
        
        for(int i=0; i<strs.length; i++){
            
            String s = strs[i];
            String sorted_s = sortString(s);
            
            if(map.containsKey(sorted_s)){
                map.get(sorted_s).add(s);
            }else{
                List<String> tempList = new ArrayList<>();
                tempList.add(s);
                map.put(sorted_s, tempList);
            }
            
        }
        
        return new ArrayList(map.values());
        
    }
    
    public String sortString(String s){
        char[] ca = s.toCharArray();
        Arrays.sort(ca);
        return String.valueOf(ca);
    }
    
}
```
