---
layout: post
title: Regular Expression Match
date: 2022-11-19 18:10 +0530
author: "Gaurav Kumar"
tags: "java interviewbit dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Implement wildcard pattern matching with support for ‘?’ and ‘*’ for strings A and B.

- ’?’ : Matches any single character.
- ‘*’ : Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

[interviewbit](https://www.interviewbit.com/problems/regular-expression-match/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/regular_expression_match.png)

```java
public class Solution {

    public int isMatch(final String s, final String p) {
        
        int n = s.length();
        
        //consequtive start can be converted to a single star
        //can be optimized by using StringBuilder
        String pattern=p.charAt(0)+"";
        for(int i=1; i<p.length(); i++){
            if(p.charAt(i) == '*' && p.charAt(i-1) == '*') continue;
            pattern += String.valueOf(p.charAt(i));
        }
        
        int m = pattern.length();
        
        int[][] dp = new int[n][m];
        for(int i=0; i<n; i++){
            Arrays.fill(dp[i], -1);
        }
        
        matchHelper(s, pattern, n-1, m-1, dp);
        
        return dp[n-1][m-1];   
    }
    
    public int matchHelper(String s, String p, int i, int j, int[][] dp){
        
        if(i == -1 && j == -1) return 1;
        
        if(j == -1){ //i is not -1, so some characters are left without any match
            return 0;
        }

        if(i == -1){ //all characters exhausted but pattern has some left

            //if remaining character in pattern are all * then it's a match
            for(int k=0; k<=j; k++){
                if(p.charAt(k) != '*') return 0;
            }

            return 1;

        }

        if(dp[i][j] == -1){

            //if characters are matching of pattern has ?
            if(s.charAt(i) == p.charAt(j) || p.charAt(j) == '?'){
                dp[i][j] = matchHelper(s, p, i-1, j-1, dp);

            //If pattern has *
            }else if(p.charAt(j) == '*'){

                //skip * as it can be empty OR match with single character and still keep it
                dp[i][j] = matchHelper(s, p, i, j-1, dp) | matchHelper(s, p, i-1, j, dp);
            }else{
                dp[i][j] = 0;
            }
        }

        return dp[i][j];

    }
    
}
```
