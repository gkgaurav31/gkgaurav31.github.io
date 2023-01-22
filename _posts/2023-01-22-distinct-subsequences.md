---
layout: post
title: Distinct Subsequences
date: 2023-01-22 17:12 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given two strings s and t, return the number of distinct
subsequences of s which equals t.

[leetcode](https://leetcode.com/problems/distinct-subsequences/description/)

### SOLUTION

```java
class Solution {

    public int numDistinct(String s, String t) {
        return distinctSequenceHelper(s, t, 0, 0, new HashMap<String, Integer>());
    }

    public int distinctSequenceHelper(String s, String t, int i, int j, Map<String, Integer> map){

        //if we have matched all characters in t, return 1
        if(j == t.length()) return 1;

        //if we didn't match all characters in t, but all characters in s are over, return 0
        if(i == s.length()) return 0;

        if(!map.containsKey(i+":"+j)){

            //init
            int count = 0;

            //if character at i matches character at j in s and t respectively
            if(s.charAt(i) == t.charAt(j)){

                //option 1: move both i and j to next character
                int x = distinctSequenceHelper(s, t, i+1, j+1, map);

                //option 2: move only i so that it can try to match again
                int y = distinctSequenceHelper(s, t, i+1, j, map);
                
                count = count + x + y;
                
            }else{

                //if they don't match, we can move i to next character to see if that can match
                count += distinctSequenceHelper(s, t, i+1, j, map);

            }

            map.put(i+":"+j, count);

        }

        return map.get(i+":"+j);

    }
}
```
