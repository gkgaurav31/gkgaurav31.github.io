---
layout: post
title: Choose and Swap (geeksforgeeks - SDE Sheet)
date: 2024-09-21 19:58 +0530
author: "Gaurav Kumar"
tags: "java greedy geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a string str of lower case english alphabets. You can choose any two characters in the string and replace all the occurences of the first character with the second character and replace all the occurences of the second character with the first character. Your aim is to find the lexicographically smallest string that can be obtained by doing this operation at most once.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/choose-and-swap0531/1?page=8)

## SOLUTION

- Find the lexicographically smallest character (minChar) in the string.
- Traverse the string to find the first character (swapChar) that is larger than minChar.
- If such a swapChar exists, swap all occurrences of minChar and swapChar.
- Return the resulting lexicographically smallest string.
