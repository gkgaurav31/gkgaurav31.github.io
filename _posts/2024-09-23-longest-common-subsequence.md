---
layout: post
title: Longest Common Subsequence (geeksforgeeks - SDE Sheet)
date: 2024-09-23 18:46 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings str1 & str 2 of length n & m respectively, return the length of their longest common subsequence. If there is no common subsequence then, return 0.

A subsequence is a sequence that can be derived from the given string by deleting some or no elements without changing the order of the remaining elements. For example, "abe" is a subsequence of "abcde".

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-common-subsequence-1587115620/1?page=9)

## SOLUTION

### BRUTE-FORCE (TLE)

```java
class Solution {
    // Function to find the length of longest common subsequence in two strings.
    static int lcs(int n, int m, String str1, String str2) {
        return lcs(str1, str2, n-1, m-1);
    }

    static int lcs(String s1, String s2, int i, int j){

        if(i < 0 || j < 0)
            return 0;

        if(s1.charAt(i) == s2.charAt(j)){
            return 1 + lcs(s1, s2, i-1, j-1);
        }

        int o1 = lcs(s1, s2, i-1, j);
        int o2 = lcs(s1, s2, i, j-1);

        return Math.max(o1, o2);

    }
}
```

### DYNAMIC PROGRAMMING (ACCEPTED)

```java
class Solution {

    static int lcs(int n, int m, String str1, String str2) {

        int[][] dp = new int[n][m];

        for(int i = 0; i < n; i++)
            Arrays.fill(dp[i], -1);

        return lcs(str1, str2, n - 1, m - 1, dp);
    }

    static int lcs(String s1, String s2, int i, int j, int[][] dp) {

        // Base case: if we reach the beginning of either string,
        // there are no more characters to compare, so return 0.
        if(i < 0 || j < 0)
            return 0;

        // If the value has not been computed yet (dp[i][j] == -1),
        // we calculate it.
        if(dp[i][j] == -1) {

            // If the characters at index i and j are the same,
            // add 1 to the result and move both i and j
            if(s1.charAt(i) == s2.charAt(j)) {
                dp[i][j] = 1 + lcs(s1, s2, i - 1, j - 1, dp);
            } else {
                // If the characters are not the same, we have two options:
                // 1. Ignore the current character of s1 (move left in DP table).
                // 2. Ignore the current character of s2 (move up in DP table).
                // Take the maximum of both options.
                int o1 = lcs(s1, s2, i - 1, j, dp);
                int o2 = lcs(s1, s2, i, j - 1, dp);

                // take the max between the above two options
                dp[i][j] = Math.max(o1, o2);
            }
        }

        return dp[i][j];
    }
}
```
