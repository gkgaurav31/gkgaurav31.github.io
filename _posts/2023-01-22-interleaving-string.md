---
layout: post
title: Interleaving String
date: 2023-01-22 01:05 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.

An interleaving of two strings s and t is a configuration where s and t are divided into n and m
substrings
respectively, such that:

```text
    s = s1 + s2 + ... + sn
    t = t1 + t2 + ... + tm
    |n - m| <= 1
    The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
```

### SOLUTION

[Tech Dose YouTube](https://www.youtube.com/watch?v=EzQ_YEmR598)

```java
class Solution {
    
    public boolean isInterleave(String s1, String s2, String s3) {
        
        if(s1.length() + s2.length() != s3.length()) return false;

        return check(s1, s2, s3, 0, 0, 0, new HashMap<String, Boolean>());
            
    }

    public boolean check(String s1, String s2, String s3, int i, int j, int k, Map<String, Boolean> map){

        //we have parsed all characters in s3
        //return true, because they must have matches some character in s1 or s2 
        if(k == s3.length()){
            return true;
        }
        
        if(!map.containsKey(i+":"+j)){

            boolean possible = false;

            //no characters in s1 remaining to be parsed
            if(i == s1.length()){
                if(s3.charAt(k) == s2.charAt(j)){
                    possible = check(s1, s2, s3, i, j+1, k+1, map);
                    map.put(i+":"+j, possible);
                    return possible;
                }else{
                    return false;
                }
            }

            //no characters in s2 remaining to be parsed
            if(j == s2.length()){
                if(s3.charAt(k) == s1.charAt(i)){
                    possible = check(s1, s2, s3, i+1, j, k+1, map);
                    map.put(i+":"+j, possible);
                    return possible;
                }else{
                    return false;
                }
            }

            boolean o1 = false;
            boolean o2 = false;
            
            if(s3.charAt(k) == s1.charAt(i)){
                o1 = check(s1, s2, s3, i+1, j, k+1, map);
            }

            if(s3.charAt(k) == s2.charAt(j)){
                o2 = check(s1, s2, s3, i, j+1, k+1, map);
            }

            map.put(i+":"+j, o1 || o2);

        }

        return map.get(i+":"+j);

    }

}
```
