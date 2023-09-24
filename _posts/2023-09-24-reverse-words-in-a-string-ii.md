---
layout: post
title: Reverse Words in a String II
date: 2023-09-24 12:40 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given a character array s, reverse the order of the words.
A word is defined as a sequence of non-space characters. The words in s will be separated by a single space.
Your code must solve the problem in-place, i.e. without allocating extra space.

[leetcode](https://leetcode.com/problems/reverse-words-in-a-string-ii/)

## SOLUTION

```java
class Solution {

    public void reverseWords(char[] s) {

        int n = s.length;

        // Step 1: Reverse the entire character array
        reverseRange(s, 0, n-1);

        // Initialize pointers i and j for word reversal
        int i=0;
        int j=0;

        // Step 2: Iterate through the character array to reverse individual words
        while(i<n){

            // Move the j pointer to the end of the current word
            while(j<n && s[j] != ' '){
                j++;
            }

            // Step 3: Reverse the current word (from i to j-1)
            reverseRange(s, i, j-1);

            // Move the i and j pointers to the next word
            i = j + 1;
            j++;
        }
    }

    // Helper function to reverse a range of characters in the array
    public void reverseRange(char[] s, int i, int j){
        while(i<j){

            // Swap characters at positions i and j
            char t = s[i];
            s[i] = s[j];
            s[j] = t;

            // Move the pointers towards each other
            i++;
            j--;
        }
    }
}
```
