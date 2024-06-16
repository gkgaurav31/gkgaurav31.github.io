---
layout: post
title: Permutation in String
date: 2022-10-27 00:21 +0530
author: "Gaurav Kumar"
tags: "java sliding_window neetcode150 important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## PROBLEM DESCRIPTION

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.  
In other words, return true if one of s1's permutations is the substring of s2.
s1 and s2 consist of lowercase English letters.

[leetcode](https://leetcode.com/problems/permutation-in-string/)

## SOLUTION

### APPROACH 1

String 2 can be a permutation of string 1, if the frequency of each element is same in both the strings. We can use an array of size 26 to keep a track of frequency of each alphabet as the input string contains only lower case alphabets.

- Pre-populate f1 array which has frequency of all the alphabets in string1
- Do the same thing for string2, but only for the first n elements where n is the size of string1. We can use another array f2 to store the freqency.
- If f1 === f2, then s1 is a permutation of f2
- We shift the windows to right. Count of element at the beginning of previous window will reduce. Count of element at the end of current window will increase.
- Compare f1 and f2 again. If both are same, return true. Otherwise, repeat the previous step
- If the window cannot be moved further (all alphabets have been exhausted in string2), return false

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {

        int n = s1.length();
        int m = s2.length();

        if(n>m) return false;

        int[] f1 = new int[26];
        int[] f2 = new int[26];

        for(int i=0; i<n; i++){
            f1[s1.charAt(i)-'a']++;
        }

        for(int i=0; i<n; i++){
            f2[s2.charAt(i)-'a']++;
        }

        if(isValidPermutation(f1, f2)) return true;

        int start=0;

        for(int end=n; end<m; end++){

            f2[s2.charAt(end)-'a']++;
            f2[s2.charAt(start)-'a']--;
            start++;

            if(isValidPermutation(f1, f2)) return true;

        }

        return false;

    }

    public boolean isValidPermutation(int[] f1, int[] f2){

        for(int i=0; i<26; i++){
            if(f1[i] != f2[i]) return false;
        }

        return true;

    }

}
```

### APPROACH 2

[NeetCode YouTube](https://www.youtube.com/watch?v=UbyhOgBN834)

If two strings are of same length, how can be determine if one is a permutation of the other?

One way is to check the frequencies of all the chars in s1 and s2. If they are exactly the same, that means they are permutations of each other. How many matches are needed? That will be equal to the number of characters allowed. In this case, the problem mentions that all are lowercase, so we need 26 matches.

We start by initializing the freq of first n characters of s1. We do this for s2 as well (for its n characters only). If the freq of all 26 chars match at this point, we can simply return true.

Otherwise, we will use sliding window to look for other substrings. We shift the window by 1 step. The freq of new char on right will increase. The freq of left most char in the previous window will decrease. Then, we can again check if the freq of all the 26 chars is exactly the same. If yes, they must be permutations of each other. So, return true.

```java
class Solution {

    public boolean checkInclusion(String s1, String s2) {

        int n = s1.length();
        int m = s2.length();

        if(n > m)
            return false;

        int[] f1 = new int[26];
        int[] f2 = new int[26];

        for(int i=0; i<n; i++)
            f1[s1.charAt(i)-'a']++;

        for(int i=0; i<n; i++) // we will iterate only till n chars since those will form the initial permutation
            f2[s2.charAt(i)-'a']++;

        if (Arrays.equals(f1, f2))
            return true;

        int idx = n; // start of next window

        while(idx < m){

            // we have moved the window one step towards right
            // so freq of char at idx should increase
            // and freq of left most char in prev window should decrease
            char newChar = s2.charAt(idx);
            char prevChar = s2.charAt(idx-n);

            f2[newChar-'a']++;
            f2[prevChar-'a']--;

            if (Arrays.equals(f1, f2))
                return true;

            idx++;

        }

        return false;

    }

    public int initializeMatch(int[] f1, int[] f2){

        int count = 0;

        for(int i=0; i<26; i++){
            if(f1[i] == f2[i])
                count++;
        }

        return count;
    }

}
```
