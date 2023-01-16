---
layout: post
title: Word Break
date: 2023-01-16 20:16 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.
Note that the same word in the dictionary may be reused multiple times in the segmentation.

[leetcode](https://leetcode.com/problems/word-break/description/)

### SOLUTION

### BRUTE-FORCE (RECURSION) - TLE

```java
class Solution {

    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>(wordDict);
        return wordHelper(s, set, 0);
    }

    public boolean wordHelper(String s, Set<String> set, int idx){

        if(idx == s.length()) return true;

        boolean possible = false;

        for(int i=idx; i<s.length(); i++){

            String substring =  s.substring(idx, i+1);
            if(set.contains(substring)){
                possible = possible || wordHelper(s, set, i+1);
            }

        }

        return possible;

    }
    
}
```

### TOP-DOWN DYNAMIC PROGRAMMING APPROACH

```java
class Solution {

    public boolean wordBreak(String s, List<String> wordDict) {
        
        Set<String> set = new HashSet<>(wordDict);

        Boolean[] dp = new Boolean[s.length()];
        Arrays.fill(dp, null);

        return wordHelper(s, set, 0, dp);

    }

    public boolean wordHelper(String s, Set<String> set, int idx, Boolean[] dp){

        //if we have processed all the characters, return true
        if(idx == s.length()) return true;

        //default value in Boolean array is null, which means it's not processed already in our case
        //so we calculate its value
        if(dp[idx] == null){

            //init
            boolean possible = false;

            //loop through each index and get the substring from index idx
            //if that substring is present in the dictionary, recursively call wordHelper for rest of the string
            for(int i=idx; i<s.length(); i++){

                String substring =  s.substring(idx, i+1);
                if(set.contains(substring)){
                    possible = possible || wordHelper(s, set, i+1, dp);
                }

            }

            dp[idx] = possible;
        }

        return dp[idx];

    }

}
```
