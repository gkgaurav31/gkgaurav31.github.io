---
layout: post
title: Longest Distinct characters in string (geeksforgeeks - SDE Sheet)
date: 2024-08-31 13:32 +0530
author: "Gaurav Kumar"
tags: "java sliding_window geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S, find the length of the longest substring with all distinct characters.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-distinct-characters-in-string5848/1?page=4)

## SOLUTION

This can be solved using sliding window. Maintain a HashSet to check the characters which have been visited.

```java
class Solution{

    static int longestSubstrDistinctChars(String s){

        int n = s.length();

        int l = 0;
        int r = 0;

        Set<Character> set = new HashSet<>();

        int maxLength = 0;

        while(r < n){

            char ch = s.charAt(r);

            if(!set.contains(ch)){

                maxLength = Math.max(maxLength, r-l+1);

                set.add(ch);

            }else{

                while(set.contains(ch)){
                    set.remove(s.charAt(l));
                    l++;
                }

                set.add(ch);

            }

            r++;

        }

        return maxLength;

    }

}
```
