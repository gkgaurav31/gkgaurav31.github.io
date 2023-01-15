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

### RECURSIVE SOLUTION (TLE)

```java
class Solution {
    
    public int numDecodings(String s) {
        return countHelper(s, 0);
    }

    public int countHelper(String s, int i){

        if(i == s.length()) return 1;
        if(s.charAt(i) == '0') return 0;

        int o1 = countHelper(s, i+1);

        int o2 = 0;
        if(i < s.length()-1){
            int x = Integer.parseInt(s.substring(i, i+2));
            if(x >= 10 && x <= 26){
                o1 += countHelper(s, i+2);
            }
        }

        return o1 + o2;
        
    }
}
```

### TOP-DOWN APPROACH WITH DP (RECURSIVE)

```java
class Solution {

    public int numDecodings(String s) {

        //init dp array
        int[] dp = new int[s.length()];
        Arrays.fill(dp, -1);

        return countHelper(s, 0, dp);
    }

    public int countHelper(String s, int i, int[] dp){

        //if i is equal to length of string, we have found 1 way
        if(i == s.length()) return 1;

        //if current char is 0, we don't have any way to decode that string
        if(s.charAt(i) == '0') return 0;

        //if we have not processed string from index i yet
        if(dp[i] == -1){

        //option 1 is to take the current character only
        //we have already checked that it's not 0.
        int o1 = countHelper(s, i+1, dp);

        //option 2 is to take current and next string only if it's between [10-26]
        int o2 = 0;
        if(i < s.length()-1){
            int x = Integer.parseInt(s.substring(i, i+2));
            if(x >= 10 && x <= 26){
                o1 += countHelper(s, i+2, dp);
            }
        }

        //save in the dp array
        dp[i] = o1 + o2;

        }

        return dp[i];
        
    }
}
```
