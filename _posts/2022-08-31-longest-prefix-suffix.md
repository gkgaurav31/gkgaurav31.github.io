---
layout: post
title: Longest Prefix Suffix
date: 2022-08-31 02:08 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing string geeksforgeeks"
categories: "string"
---

## Problem Description

Given a string of characters, find the length of the longest proper prefix which is also a proper suffix.
NOTE: Prefix and suffix can be overlapping but they should not be equal to the entire string.  
[geeksforgeeks](https://practice.geeksforgeeks.org/problems/longest-prefix-suffix2527/1)

> Example:  
> Input: s = "abab"  
> Output: 2  
> Explanation: "ab" is the longest proper  
> prefix and suffix.  

### Solution

```java
class Solution {
    
    int lps(String s) {
        
        int[] lps = new int[s.length()];
        lps[0] = 0;
        int maxLength = 0;
        
        for(int i=1; i<s.length(); i++){
            
            int x = lps[i-1];
            while(s.charAt(i) != s.charAt(x)){
                if(x == 0){
                    x=-1;
                    break;
                }
                x = lps[x-1];
            }
            
            lps[i] = x+1;
            if(lps[i] > maxLength) maxLength = lps[i];
            
        }
        
        return lps[s.length()-1];
        
    }
}
```
