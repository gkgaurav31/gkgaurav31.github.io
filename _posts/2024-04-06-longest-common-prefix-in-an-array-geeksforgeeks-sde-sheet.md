---
layout: post
title: Longest Common Prefix in an Array (geeksforgeeks - SDE Sheet)
date: 2024-04-06 12:48 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of N strings, find the longest common prefix among all strings present in the array. Return "-1" if there is no common prefix.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-common-prefix-in-an-array5129/1?page=1)

## SOLUTION

```java
class Solution{

    String longestCommonPrefix(String arr[], int n){

        // Initialize the longest common prefix with the first string in the array
        String lcp = arr[0];

        // Iterate through the array of strings starting from the second string
        for(int i=1; i<n; i++){

            // Update the longest common prefix by finding the common prefix between the current prefix and the next string
            lcp = commonPrefix(lcp, arr[i]);

        }

        // If no common prefix is found, return "-1", otherwise return the longest common prefix
        return lcp.equals("") ? "-1" : lcp.toString();

    }

    // Method to find the common prefix between two strings
    String commonPrefix(String s1, String s2){

        // Create a StringBuffer to store the common prefix
        StringBuffer sb = new StringBuffer();

        int i = 0;

        // Iterate through the strings as long as there are characters left in both strings and the characters are equal
        while(i < s1.length() && i < s2.length() && s1.charAt(i) == s2.charAt(i)){
            // Append the current character to the common prefix, and move to next char
            sb.append(String.valueOf(s1.charAt(i)));
            i++;
        }

        return sb.toString();

    }

}
```
