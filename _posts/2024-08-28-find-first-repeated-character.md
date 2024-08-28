---
layout: post
title: Find first repeated character (geeksforgeeks - SDE Sheet)
date: 2024-08-28 22:29 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S. The task is to find the first repeated character in it. We need to find the character that occurs more than once and whose index of second occurrence is smallest. S contains only lowercase letters.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-first-repeated-character4108/1?page=3)

## SOLUTION

```java
class Solution
{
    String firstRepChar(String s)
    {

        int[] f = new int[26];

        for(int i=0; i<s.length(); i++){

            char ch = s.charAt(i);

            if(f[ch-'a'] != 0)
                return String.valueOf(ch);

            f[ch-'a']++;

        }

        return "-1";

    }
}
```
