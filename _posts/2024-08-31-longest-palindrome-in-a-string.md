---
layout: post
title: Longest Palindrome in a String (geeksforgeeks - SDE Sheet)
date: 2024-08-31 17:40 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string str, find the longest palindromic substring in str. Substring of string str: str[ i . . . . j ] where 0 ≤ i ≤ j < len(str). Return the longest palindromic substring of str.

Palindrome string: A string that reads the same backward. More formally, str is a palindrome if reverse(str) = str. In case of conflict, return the substring which occurs first ( with the least starting index).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-palindrome-in-a-string3411/1?page=4)

## SOLUTION

At each position, assume it's the mid and try to form the largest palindrome using two pointers left and right. We may have odd or even length palindromes and they can be handled separately.

```java
class Solution{

    static String longestPalin(String s){

        int n = s.length();
        int maxLength = 1;

        int l = 0;
        int r = 0;

        int resL = 0;
        int resR = 0;

        for(int i=0; i<n; i++){

            // odd
            l = i;
            r = i;

            while(l >= 0 && r < n && s.charAt(l) == s.charAt(r)){
                l--;
                r++;
            }

            if(r-l-1 > maxLength){

                resL = l + 1;
                resR = r - 1;

                maxLength = Math.max(maxLength, resR - resL + 1);
            }


            // even
            if(i < n-1 && s.charAt(i) == s.charAt(i+1)){

                l = i;
                r = i+1;

                while(l >= 0 && r < n && s.charAt(l) == s.charAt(r)){
                    l--;
                    r++;
                }

                if(r-l-1 > maxLength){

                    resL = l + 1;
                    resR = r - 1;

                    maxLength = Math.max(maxLength, resR - resL + 1);
                }


            }

        }

        return s.substring(resL, resR + 1);

    }

}
```
