---
layout: post
title: Minimum indexed character (geeksforgeeks - SDE Sheet)
date: 2024-08-28 22:27 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string str and another string patt. Find the minimum index of the character in str that is also present in patt.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-indexed-character-1587115620/1?page=3)

## SOLUTION

```java
class Solution
{
    //Function to find the minimum indexed character.
    public static int minIndexChar(String str, String patt)
    {
        Set<Character> set = new HashSet<>();
        for(int i=0; i<patt.length(); i++)
            set.add(patt.charAt(i));

        for(int i=0; i<str.length(); i++){
            if(set.contains(str.charAt(i)))
                return i;
        }

        return -1;
    }
}
```
