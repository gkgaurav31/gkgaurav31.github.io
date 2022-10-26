---
layout: post
title: Longest Repeating Character Replacement
date: 2022-10-26 23:51 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## Problem Description

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.  
Return the length of the longest substring containing the same letter you can get after performing the above operations.  

Example:  

Input: s = "ABAB", k = 2  
Output: 4  
Explanation: Replace the two 'A's with two 'B's or vice versa.  

[leetcode](https://leetcode.com/problems/longest-repeating-character-replacement/)

### Solution

The main logic is that, if we have any string and the frequency of the element which occurs most numbers of times is X, then the number of operations needed to change other alphabets is = length of substring - X. If this is > k, then this is not a valid substring.

```java
class Solution {
    
    public int characterReplacement(String s, int k) {
        
        int n = s.length();
        
        //We will use this to store the frequency of each upper-case alphabet
        //Note that the question mentions that the input string contains only uppercase alphabets
        int[] frequency = new int[26];
        
        int start = 0;
        
        int maxLength = 0;
        int maxFrequency = 0;
        
        //We will start the window from [0,0]
        for(int end=0; end<n; end++){
            
            //Get the current character
            char current = s.charAt(end);
            
            //Increase the frequency of that character
            //Here, A is at index 0, B is at index 1, C is at index 2, and so on...
            frequency[current-'A']++;
            
            //This part does the main trick for us. We maintain the max frequency while iterating the characters
            //The main idea is: If the total length is x and the frequency of max occurring element in that substring is y
            //then the number of changes required is: x-y
            maxFrequency = Math.max(maxFrequency, frequency[current-'A']);
            
            //Same as x-y explained above. 
            //Length of substring minus frequency of most occurring element
            int diff = (end-start+1) - maxFrequency;
            
            //If the difference is more than k, that substring is not a valid answer
            //So, we decrease the count of start of the window
            //And shift start to right
            if(diff > k){
                frequency[s.charAt(start)-'A']--;
                start++;
            }
            
            //Update max length. Right of the windows will increase due to the for loop condition which is what we want
            maxLength = Math.max(maxLength, end-start+1);
            
        }
        
        return maxLength;
        
    }
    
}
```
