---
layout: post
title: Palindromic Substrings
date: 2022-11-21 20:27 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 leetcode dynamic_programming"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given a string s, return the longest palindromic substring in s.

[leetcode](https://leetcode.com/problems/longest-palindromic-substring/description/)

### Solution

This question is similar to: [Longest Palindrome SubString]({% post_url 2022-11-20-longest-palindromic-substring%})

```java
class Solution {
    
    public int countSubstrings(String s) {
        
        int n = s.length();
        int count = 0;

        for(int i=0; i<n; i++){

            //odd palindrome
            int oddCount = countPalindromes(s, i, i);
            
            //even palindrome
            int evenCount = countPalindromes(s, i, i+1);

            count = count + oddCount + evenCount;

        }

        return count;

    }

    public int countPalindromes(String s, int start, int end){

        int count = 0;
        
        int l = start;
        int r = end;

        while(l>=0 && r<s.length() && s.charAt(l) == s.charAt(r)){
            count++;
            l--;
            r++;
        }
        
        return count;

    }

}
```
