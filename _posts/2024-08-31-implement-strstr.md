---
layout: post
title: Implement strstr (geeksforgeeks - SDE Sheet)
date: 2024-08-31 11:43 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Your task is to implement the function strstr. The function takes two strings as arguments (s,x) and locates the occurrence of the string x in the string s. The function returns an integer denoting the first occurrence of the string x in s (0 based indexing).

Note: You are not allowed to use inbuilt function.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/implement-strstr/1?page=4)

## SOLUTION

```java
class GfG
{
    int strstr(String s, String x)
    {

        // Start from index 0 of string s
        int idx = 0;

        // If the length of x is greater than s, x cannot be a substring of s.
        if(x.length() > s.length())
            return -1;

        // Iterate through string s using idx as the starting index
        while(idx < s.length()){

            // Check if the current character in s matches the first character of x
            if(s.charAt(idx) == x.charAt(0)){

                // Pointer for s starting from current index idx
                // Pointer for x starting from the beginning
                int i = idx;
                int j = 0;

                // Check subsequent characters in both strings
                while(i < s.length() && j < x.length()){

                    // If characters do not match, break out of the loop
                    if(s.charAt(i) != x.charAt(j))
                        break;

                    i++;  // Move to next character in s
                    j++;  // Move to next character in x

                }

                // If we have compared all characters of x (j == x.length()),
                // it means x is found in s starting from index idx
                if(j == x.length())
                    // Return the starting index of the found substring
                    return idx;

            }

            // Increment idx to check for the substring from the next character in s
            idx++;

        }

        // If the loop completes without returning, x was not found in s
        return -1;
    }
}
```

> THIS CAN ALSO BE SOLVED USING [RABIN KARP ALGORITHM](https://gkgaurav31.github.io/posts/rabin-karp/)
