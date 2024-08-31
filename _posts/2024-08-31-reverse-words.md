---
layout: post
title: Reverse Words (geeksforgeeks - SDE Sheet)
date: 2024-08-31 12:30 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a String S, reverse the string without reversing its individual words. Words are separated by dots.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/reverse-words-in-a-given-string5459/1?page=4)

## SOLUTION

### APPROACH 1 (TWO POINTERS)

```java
class Solution
{

    String reverseWords(String s)
    {
        int n = s.length();

        // StringBuffer to store the reversed words
        StringBuffer sb = new StringBuffer();

        int i = n - 1; // Start from the end of the string
        int j = n - 1; // 'j' will mark the end of the current word while traversing from right to left

        // Traverse the string from the end to the beginning
        while(i >= 0)
        {
            // Move 'i' backwards to find the beginning of a word or the start of the string
            while(i >= 0 && s.charAt(i) != '.')
            {
                i--;
            }

            // Add the current word to the StringBuffer
            for(int k = i + 1; k <= j; k++)
            {
                sb.append(String.valueOf(s.charAt(k)));
            }

            // Move 'i' one step left to skip the dot
            // j will also move to the same position
            i--;
            j = i;

            sb.append(".");
        }

        // Remove the last dot that was added after the last word and return the result
        return sb.toString().substring(0, n);
    }
}
```

### APPROACH 2 (APPEND TO LEFT)

```java
class Solution
{
    String reverseWords(String s)
    {
        int n = s.length();

        // res: store the final result
        // current: accumulate characters of the current word
        String res = "";
        String current = "";

        for(int i = 0; i < n; i++)
        {
            char ch = s.charAt(i);

            // Check if the current character is a dot, indicating the end of a word.
            if(ch == '.')
            {
                // Prepend the current word and dot to 'res'.
                res = "." + current + res;

                // Reset 'current' to start accumulating the next word.
                current = "";
            }
            else
            {
                // If it's not a dot, add the character to 'current' to form the current word.
                current += ch;
            }
        }

        // After the loop, 'current' contains the last word. Add it to 'res' without a preceding dot.
        res = current + res;

        return res;
    }
}
```
