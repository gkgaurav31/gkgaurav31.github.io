---
layout: post
title: Reverse words in a given string (geeksforgeeks - SDE Sheet)
date: 2024-04-16 20:08 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks strings"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a String S, reverse the string without reversing its individual words. Words are separated by dots.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/reverse-words-in-a-given-string5459/1?page=3)

## SOLUTION

A simple way to solve this is by using split() method. Remember to escape the special dot character `.` 🙃

Here, we will solve this without using split(). The idea is simple, maintain two pointers - one to mark the start and another to mark the end of any word. We can start from the right of the string. Keep looking for the next dot while shifting the left pointers towards left. When the left pointers meets the dot, we can say that the word is from index [left+1, right]. For the next word, decrease left by 1 and reset right to the same position as left.

Continue doing this until left is less than 0 (no more characters left).

```java
class Solution
{
    String reverseWords(String S)
    {
        int n = S.length();

        // StringBuffer to store the reversed string
        StringBuffer sb = new StringBuffer();

        // Initialize pointers for traversing the string
        int left = n - 1; // Points to the end of the current word
        int right = n - 1; // Points to the end of the string

        // Loop until we have processed the entire string
        while (left >= 0) {
            // Move left pointer until we reach the beginning of the current word
            while (left >= 0 && S.charAt(left) != '.') {
                left--;
            }

            // Append the current word (from left to right) to the StringBuffer
            sb.append(S.substring(left + 1, right + 1) + ".");

            // Move left pointer one step back to skip the dot
            left--;
            // Update right pointer to the new end of the next word
            right = left;
        }

        // Convert StringBuffer to String and remove the trailing dot
        return sb.toString().substring(0, sb.length() - 1);
    }
}
```
