---
layout: post
title: Longest Palindromic Substring
date: 2022-11-20 22:43 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 leetcode dynamic_programming"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given a string s, return the longest palindromic substring in s.

[leetcode](https://leetcode.com/problems/longest-palindromic-substring/description/)

### Solution

```java
class Solution {

    //Time complexity: O(n^2)
    public String longestPalindrome(String s) {

        String ans = "";

        //At every index, check for longest palindrome substring
        for(int i=0; i<s.length(); i++){

            //odd length
            String oddString = palindromeHelper(s, i, i);
            if(oddString.length() > ans.length()){
                ans = oddString;
            }

            //even length
            String evenString = palindromeHelper(s, i, i+1);
            if(evenString.length() > ans.length()){
                ans = evenString;
            }

        }

        return ans;

    }

    public String palindromeHelper(String s, int l, int r){

        int start = -1;
        int end = -1;

        while(l>=0 && r<s.length() && s.charAt(l) == s.charAt(r)){
            start = l;
            end = r;
            l--;
            r++;
        }

        if(start == -1) return "";

        return s.substring(l+1, r);

    }

}
```

> Further Reading:
> [Manacher's algorithm | O(n) solution](https://en.wikipedia.org/wiki/Longest_palindromic_substring#Manacher's_algorithm)
