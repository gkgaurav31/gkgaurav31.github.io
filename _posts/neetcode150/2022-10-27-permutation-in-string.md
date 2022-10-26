---
layout: post
title: Permutation in String
date: 2022-10-27 00:21 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## Problem Description

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.  
In other words, return true if one of s1's permutations is the substring of s2.
s1 and s2 consist of lowercase English letters.

[leetcode](https://leetcode.com/problems/permutation-in-string/)

### Solution

String 2 can be a permutation of string 1, if the frequency of each element is same in both the strings. We can use an array of size 26 to keep a track of frequency of each alphabet as the input string contains only lower case alphabets.

- Pre-populate f1 array which has frequency of all the alphabets in string1
- Do the same thing for string2, but only for the first n elements where n is the size of string1. We can use another array f2 to store the freqency.
- If f1 === f2, then s1 is a permutation of f2
- We shift the windows to right. Count of element at the beginning of previous window will reduce. Count of element at the end of current window will increase.
- Compare f1 and f2 again. If both are same, return true. Otherwise, repeat the previous step
- If the window cannot be moved further (all alphabets have been exhausted in string2), return false

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        
        int n = s1.length();
        int m = s2.length();
        
        if(n>m) return false;
        
        int[] f1 = new int[26];
        int[] f2 = new int[26];
        
        for(int i=0; i<n; i++){
            f1[s1.charAt(i)-'a']++;
        }
        
        for(int i=0; i<n; i++){
            f2[s2.charAt(i)-'a']++;
        }
        
        if(isValidPermutation(f1, f2)) return true;
        
        int start=0;
        
        for(int end=n; end<m; end++){
            
            f2[s2.charAt(end)-'a']++;
            f2[s2.charAt(start)-'a']--;
            start++;
            
            if(isValidPermutation(f1, f2)) return true; 
            
        }
        
        return false;
        
    }
    
    public boolean isValidPermutation(int[] f1, int[] f2){
        
        for(int i=0; i<26; i++){
            if(f1[i] != f2[i]) return false;
        }
        
        return true;
        
    }
    
}
```
