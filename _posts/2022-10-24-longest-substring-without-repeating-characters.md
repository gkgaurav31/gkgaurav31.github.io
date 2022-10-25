---
layout: post
title: Longest Substring Without Repeating Characters
date: 2022-10-24 14:17 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given a string s, find the length of the longest substring without repeating characters.

[leetcode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### Solution

This can be solved using sliding window technique. The idea is to main a HashSet to keep a track of elements which have been "visited". If the current element has not been visited, we increase the right end of the window. We add that element to the hashset.  

If the element has been already been visited (hashset already contains it), we can reduce the start of the window one by one and one and keep removing the element on left most side from the hashset. One the previous occurance of the current element has been removed, we will continue by extending the right on the window as in the previous step.  

This can be further optimized by keeping a track of index of last occurance of any element.

```java
class Solution {
    
    public int lengthOfLongestSubstring(String s) {
        
        Set<String> set = new HashSet<>();
        
        int n = s.length();
        
        int maxLength=0;
        
        int start = 0;
        int e=0;
        
        while(e<n){
            
            String c = s.charAt(e) + "";
            
            if(!set.contains(c)){
                set.add(c);
                e++;
                maxLength = Math.max(e-start, maxLength);
            }else{
                set.remove(s.charAt(start)+"");
                start++;
            }
            
        }
        
        return maxLength;
    }
    
}
```
