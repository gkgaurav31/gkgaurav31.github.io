---
layout: post
title: Recursively remove all adjacent duplicates (geeksforgeeks - SDE Sheet)
date: 2024-09-01 15:36 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string s, remove all its adjacent duplicate characters recursively.

Note: For some test cases, the resultant string would be an empty string. In that case, the function should return the empty string only.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/recursively-remove-all-adjacent-duplicates0744/1?page=4)

## SOLUTION

### APPROACH 1 - BRUTE FORCE (ACCEPTED)

Iterate through all the character of s and generate another string which remove the duplicates in the first iteration. The resultant string can be the input for recursive call. If the resultant string is the same as original string, that means there were no duplicates and we can simply return that string.

```java
class Solution {

    String rremove(String s) {

        int n = s.length();

        // Base case: If the string length is 0 or 1, it is returned as is
        if (n <= 1)
            return s;

        // Use a StringBuilder to construct the string after removing adjacent duplicates
        StringBuilder sb = new StringBuilder();

        int i = 0;

        // Iterate through the string to find and remove adjacent duplicates
        while (i < n - 1) {

            // If current character is the same as the next one, skip all duplicates
            if (s.charAt(i) == s.charAt(i + 1)) {

                int j = i;

                // Find the end of the duplicate sequence
                while (j < n && s.charAt(i) == s.charAt(j)) {
                    j++;
                }

                // Move the index to the end of the duplicate sequence
                i = j;

            } else {

                // If no duplicates, append the current character to the result
                sb.append(s.charAt(i));
                i++;

            }

        }

        // Handle the last character if it's not part of a duplicate sequence
        if (n > 1 && s.charAt(n - 1) != s.charAt(n - 2)) {
            sb.append(s.charAt(n-1));
        }

        // If no characters were removed (result is the same as input), return the result
        if (s.equals(sb.toString())) // we could also use length
            return s;

        // Recursively process the result string until no more adjacent duplicates are found
        return rremove(sb.toString());
    }
}
```
