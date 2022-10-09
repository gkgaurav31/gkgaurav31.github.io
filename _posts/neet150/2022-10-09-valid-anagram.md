---
layout: post
title: Valid Anagram
date: 2022-10-09 14:04 +0530
author: "Gaurav Kumar"
tags: "java neet150"
categories: "neet150"
---

This question is part of [Neet150 series](https://neetcode.io/practice).  

## Problem Description

Given two strings s and t, return true if t is an anagram of s, and false otherwise.
An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

[leetcode](https://leetcode.com/problems/valid-anagram/)

### Solution

```java
class Solution {
    
    public boolean isAnagram(String s, String t) {
        
        Map<Character, Integer> map = new HashMap<>();
        
        for(int i=0; i<s.length(); i++){
            
            Character c = s.charAt(i);
            
            if(map.containsKey(c)){
                int frequency = map.get(c);
                map.put(c, frequency+1);
            }else{
                map.put(c, 1);
            }
            
        }
        
        for(int i=0; i<t.length(); i++){
            
            Character x = t.charAt(i);
            
            if(!map.containsKey(x)){
                return false;
            }else{
                int frequency = map.get(x);
                if(frequency == 1){
                    map.remove(x);
                }else{
                    map.put(x, frequency-1);
                }
            }
            
        }
        
        return map.size()==0?true:false;
        
    }
}
```
