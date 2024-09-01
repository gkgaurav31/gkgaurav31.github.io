---
layout: post
title: Form a palindrome (geeksforgeeks - SDE Sheet)
date: 2024-09-01 18:35 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string, find the minimum number of characters to be inserted to convert it to a palindrome.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/form-a-palindrome1455/1?page=5)

## SOLUTION

[Good Explanation - takeUForward](https://www.youtube.com/watch?v=xPBLEj41rFU)

To understand how we calculate longest common subsequence, refer:  
[Longest Common Subsequence]({% post_url 2022-11-12-longest-common-subsequence %})

The main idea is to take the reverse of the given string and find the longest common subsequence between them.

The minimum number of characters which need to be added to make the original string a palindrome them becomes:

`(length of str) - (length of the longest palindrom subsequence)`

### BRUTE-FORCE WITH LCS (TLE)

```java
class Solution{

    static int countMin(String str)
    {

        int n = str.length();

        String reverseStr = new StringBuilder(str).reverse().toString();

        return n - longestCommonSubsequence(str, reverseStr, n-1, n-1);

    }

    static int longestCommonSubsequence(String s1, String s2, int i, int j){

        if(i < 0 || j < 0)
            return 0;

        if(s1.charAt(i) == s2.charAt(j)){
            return 1 + longestCommonSubsequence(s1, s2, i-1, j-1);
        }

        return Math.max(longestCommonSubsequence(s1, s2, i-1, j), longestCommonSubsequence(s1, s2, i, j-1));

    }

}
```

### MEMOIZED USING HASHMAP (TLE)

```java
class Solution{

    static int countMin(String str)
    {

        int n = str.length();

        String reverseStr = new StringBuilder(str).reverse().toString();

        return n - longestCommonSubsequence(str, reverseStr, n-1, n-1, new HashMap<>());

    }

    static int longestCommonSubsequence(String s1, String s2, int i, int j, Map<String, Integer> map){

        if(i < 0 || j < 0)
            return 0;

        String key = i + ":" + j;

        if(map.containsKey(key))
            return map.get(key);

        if(s1.charAt(i) == s2.charAt(j)){
            return 1 + longestCommonSubsequence(s1, s2, i-1, j-1, map);
        }

        map.put(key, Math.max(longestCommonSubsequence(s1, s2, i-1, j, map), longestCommonSubsequence(s1, s2, i, j-1, map)));

        return map.get(key);

    }

}
```

### MEMOIZED USING 2D DP MATRIX (ACCEPTED)

```java
class Solution{

    static int countMin(String str)
    {

        int n = str.length();

        String reverseStr = new StringBuilder(str).reverse().toString();

        int[][] dp = new int[n][n];
        for(int i=0; i<n; i++)
            Arrays.fill(dp[i], -1);

        return n - longestCommonSubsequence(str, reverseStr, n-1, n-1, dp);

    }

    static int longestCommonSubsequence(String s1, String s2, int i, int j, int[][] dp){

        if(i < 0 || j < 0)
            return 0;

        if(dp[i][j] == -1){

            if(s1.charAt(i) == s2.charAt(j)){
                dp[i][j] = 1 + longestCommonSubsequence(s1, s2, i-1, j-1, dp);
            }else{
                dp[i][j] = Math.max(longestCommonSubsequence(s1, s2, i-1, j, dp), longestCommonSubsequence(s1, s2, i, j-1, dp));
            }

        }

        return dp[i][j];

    }

}
```
