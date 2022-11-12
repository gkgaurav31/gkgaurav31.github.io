---
layout: post
title: Longest Common Subsequence
date: 2022-11-12 21:29 +0530
author: "Gaurav Kumar"
tags: "java leetcode strings dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.  

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.  

For example, "ace" is a subsequence of "abcde".  

A common subsequence of two strings is a subsequence that is common to both strings.  
[leetcode](https://leetcode.com/problems/longest-common-subsequence/description/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/longest_common_subsequence.png)

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        
        int n = text1.length();
        int m = text2.length();

        //initialize all to -1
        int[][] dp = new int[n][m];
        for(int i=0; i<n; i++){
            Arrays.fill(dp[i], -1);
        }

        return LCS(text1, text2, n-1, m-1, dp);
    }

    public int LCS(String s1, String s2, int i, int j, int[][] dp){
        
        //one of the strings are empty since end index is -1
        if(i == -1 || j == -1) return 0;

        //dp[i][j] has not been calculated
        if(dp[i][j] == -1){
            
            //if characters at ith index of string1 and jth index of string2 are same
            if(s1.charAt(i) == s2.charAt(j)){

                //the result will be LCS of rest of the string for both plus 1 (since we found a common character)
                dp[i][j] = 1 + LCS(s1, s2, i-1, j-1, dp);

            }else{

                //otherwise, ignore ith character of string1 and get the LCS. Do the same thing by ignoring jth character of string1. 
                //take the maximum between these two options
                dp[i][j] = Math.max(LCS(s1, s2, i-1, j, dp), LCS(s1, s2, i, j-1, dp));
            }
        }

        return dp[i][j];

    }

}
```
