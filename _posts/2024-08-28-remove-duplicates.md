---
layout: post
title: Remove Duplicates
date: 2024-08-28 22:12 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string str without spaces, the task is to remove all duplicate characters from it, keeping only the first occurrence.

Note: The original order of characters must be kept the same.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/remove-duplicates3034/1?page=3)

## SOLUTION

```java
class Solution {

    String removeDups(String str) {

        int[] chars = new int[26];

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<str.length(); i++){

            char ch = str.charAt(i);

            if(chars[ch - 'a'] == 0){
                sb.append(ch + "");
                chars[ch - 'a'] = 1;
            }

        }

        return sb.toString();

    }

}
```
