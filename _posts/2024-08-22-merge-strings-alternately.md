---
layout: post
title: Merge Strings Alternately
date: 2024-08-22 22:01 +0530
author: "Gaurav Kumar"
tags: "java strings leetcode microsoft"
categories: "microsoft30days"
---

## Problem Description

You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

[leetcode](https://leetcode.com/problems/merge-strings-alternately/description/?envType=company&envId=microsoft&favoriteSlug=microsoft-thirty-days)

## Solution

```java
class Solution {

    public String mergeAlternately(String word1, String word2) {

        StringBuffer sb = new StringBuffer();

        int idx = 0;

        int n = word1.length();
        int m = word2.length();

        while(idx < n && idx < m){

            sb.append(String.valueOf(word1.charAt(idx)));
            sb.append(String.valueOf(word2.charAt(idx)));

            idx++;

        }

        while(idx < n)
            sb.append(String.valueOf(word1.charAt(idx++)));

        while(idx < m)
            sb.append(String.valueOf(word2.charAt(idx++)));

        return sb.toString();

    }

}
```
