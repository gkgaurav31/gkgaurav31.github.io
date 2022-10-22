---
layout: post
title: Valid Palindrome
date: 2022-10-22 21:14 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

[leetcode](https://leetcode.com/problems/valid-palindrome/)

### Solution

### Two Pointer Approach

```java
class Solution {
    public boolean isPalindrome(String s) {
        
        s = s.toLowerCase();
        
        int i=0;
        int j=s.length()-1;
        
        while(i<j){
            
            if(!Character.isLetterOrDigit(s.charAt(i))){ i++; continue;}
            if(!Character.isLetterOrDigit(s.charAt(j))){ j--; continue;}
            
            if(s.charAt(i) != s.charAt(j)) return false;
            
            i++; j--;
        }
        
        return true;
        
    }
}
```
