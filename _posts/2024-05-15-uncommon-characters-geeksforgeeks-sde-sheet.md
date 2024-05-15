---
layout: post
title: Uncommon characters (geeksforgeeks - SDE Sheet)
date: 2024-05-15 22:24 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks strings sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings A and B consisting of lowercase english characters. Find the characters that are not common in the two strings.

Return the string in sorted order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/uncommon-characters4932/1?page=3)

## SOLUTION

```java
class Solution
{
    String UncommonChars(String A, String B)
    {

        int[] f1 = new int[26];
        for(int i=0; i<A.length(); i++){
            f1[A.charAt(i)-'a'] = 1;
        }

        int[] f2 = new int[26];
        for(int i=0; i<B.length(); i++){
            f2[B.charAt(i)-'a'] = 1;
        }

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<26; i++){

            if((f1[i]^f2[i]) == 1){
                sb.append(String.valueOf((char)(i+'a')));
            }

        }

        return sb.toString().equals("")?"-1":sb.toString();

    }
}
```
