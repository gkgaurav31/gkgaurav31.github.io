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

### BOTTOM-DOWN DYNAMIC PROGRAMMING APPROACH

[NeetCode YouTube](https://www.youtube.com/watch?v=Sx9NNgInc3A)

```java
class Solution {

    public boolean wordBreak(String s, List<String> wordDict) {

        int n = s.length();

        // Create a dynamic programming (DP) array of size 'n+1'.
        // dp[i] represents whether the substring from index 'i' to the end of 's' can be segmented into words from 'wordDict'.
        boolean[] dp = new boolean[n + 1];

        // Initialize the last element of 'dp' as true because an empty string can be
        // segmented into words trivially.
        dp[n] = true;

        // Convert 'wordDict' into a HashSet for efficient lookup and remove duplicates if any. (this is not necessary though)
        Set<String> set = new HashSet<>(wordDict);

        // Start iterating through 's' in reverse order (from right to left).
        for (int i = n - 1; i >= 0; i--) {

            // Iterate through each word 'x' in 'wordDict'.
            for (String x : set) {

                int lengthOfWord = x.length();

                // If the length of 'x' is greater than the remaining substring's length,
                // skip this iteration, as it's not possible to match 'x' with the remaining substring.
                if (lengthOfWord > n - i) {
                    continue;
                }

                // Otherwise, check if 'x' is equal to the substring of 's' starting at index 'i' and ending at 'i + length of current word'.
                if (x.equals(s.substring(i, i + lengthOfWord))) {

                    // If 'x' matches the substring, update dp[i] with dp[i + lengthOfWord].
                    // The idea is that if the current word matches the substring, and the rest of the substring starting at i+lengthOfWord the result was true
                    // then we can mark the current state as true
                    dp[i] = dp[i + lengthOfWord];

                    // If dp[i] becomes true, it means we found a valid segmentation,
                    // so we can break out of this inner loop. (if we don't do it, there is a chance that for later words it will change it back to false)
                    if (dp[i] == true) {
                        break;
                    }
                }
            }
        }

        // The final value of dp[0] will indicate whether the entire string 's'
        // can be segmented into words from 'wordDict'.
        return dp[0];
    }
}
```
