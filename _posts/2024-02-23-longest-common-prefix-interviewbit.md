---
layout: post
title: Longest Common Prefix (InterviewBit)
date: 2024-02-23 19:43 +0530
author: "Gaurav Kumar"
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given the array of strings `A`, you need to find the longest string `S` which is the prefix of ALL the strings in the array.

Longest common prefix for a pair of strings `S1` and `S2` is the longest string `S` which is the prefix of both `S1` and `S2`.

For Example: longest common prefix of "abcdefgh" and "abcefgh" is "abc".

## SOLUTION

### APPROACH 1

```java
public class Solution {

    public String longestCommonPrefix(String[] A) {

        int n = A.length;

        if(n == 0)
            return  "";

        String commonPf = A[0];

        for(int i=1; i<n; i++){
            commonPf = common(A[i], commonPf);
        }

        return commonPf.toString();

    }

    public String common(String s1, String s2){

        int n = s1.length();
        int m = s2.length();

        StringBuffer sb = new StringBuffer();

        int idx = 0;

        while(idx < n && idx < m && s1.charAt(idx) == s2.charAt(idx)){
            sb.append(String.valueOf(s1.charAt(idx)));
            idx++;
        }

        return sb.toString();

    }

}
```

### APPROACH 2

Sort the array (it will be sorted lexographically). Then we just need to find the prefix between first and last element.

```java
public class Solution {

    public String longestCommonPrefix(String[] A) {

        int n = A.length;

        if(n == 0)
            return  "";

        Arrays.sort(A);

        return common(A[0], A[n-1]);

    }

    public String common(String s1, String s2){

        int n = s1.length();
        int m = s2.length();

        StringBuffer sb = new StringBuffer();

        int idx = 0;

        while(idx < n && idx < m && s1.charAt(idx) == s2.charAt(idx)){
            sb.append(String.valueOf(s1.charAt(idx)));
            idx++;
        }

        return sb.toString();

    }

}
```
