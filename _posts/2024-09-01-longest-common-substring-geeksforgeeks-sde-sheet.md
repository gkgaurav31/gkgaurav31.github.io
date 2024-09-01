---
layout: post
title: Longest Common Substring (geeksforgeeks - SDE Sheet)
date: 2024-09-01 12:47 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given two strings str1 and str2. Your task is to find the length of the longest common substring among the given strings.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-common-substring1452/1?page=4)

## SOLUTION

### APPROACH 1 - BRUTE FORCE (ACCEPTED)

```java
class Solution {

    public int longestCommonSubstr(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        // maximum length of the common substring
        int maxLength = 0;

        // Iterate through each character in str1
        for(int i = 0; i < n; i++) {

            // Iterate through each character in str2
            for(int j = 0; j < m; j++) {

                // Check if the current characters in str1 and str2 match
                if(str1.charAt(i) == str2.charAt(j)) {

                    // counter to keep track of the length of the current matching substring
                    int c = 0;

                    // two pointers to traverse both strings from the current matching position
                    int l = i;
                    int r = j;

                    // Continue checking characters in both strings while they match and are within bounds
                    while(l < n && r < m && str1.charAt(l) == str2.charAt(r)) {
                        c++;
                        l++;
                        r++;
                    }

                    maxLength = Math.max(maxLength, c);
                }
            }
        }

        return maxLength;
    }
}
```
