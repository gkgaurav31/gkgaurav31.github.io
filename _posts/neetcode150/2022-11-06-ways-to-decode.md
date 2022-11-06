---
layout: post
title: Ways to Decode
date: 2022-11-06 23:28 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150 important"
categories: "neetcode150"
---

## Problem Description

A message containing letters from A-Z can be encoded into numbers using the following mapping:

```plaintext
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

- "AAJF" with the grouping (1 1 10 6)
- "KJF" with the grouping (11 10 6)

Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".  

Given a string s containing only digits, return the number of ways to decode it.  

The test cases are generated so that the answer fits in a 32-bit integer.  

[leetcode](https://leetcode.com/problems/decode-ways/description/)

### Solution

```java
class Solution {

    public int numDecodings(String A) {
        int n = A.length();

        if(n == 0) return 0;
        if(A.charAt(0) == '0') return 0;

        int dp[] = new int[n+1];

        dp[0] = 1;
        dp[1] = 1;

        for(int i=2; i<=n; i++){
            
            int c = A.charAt(i-1)-'0';
            
            if(c >= 1 && c<=9){
                dp[i] = dp[i-1];
            }
            
            int withPrevious = Integer.parseInt(A.substring(i-2, i)); 

            if(withPrevious >= 10 && withPrevious <= 26){
                dp[i] = dp[i] + dp[i-2];
            }

        }

        return dp[n];

    }
}
```
