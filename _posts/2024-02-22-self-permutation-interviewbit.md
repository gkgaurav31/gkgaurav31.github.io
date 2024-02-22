---
layout: post
title: Self Permutation (InterviewBit)
date: 2024-02-22 20:22 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given two strings A and B.
Check whether there exists any permutation of both A and B such that they are equal.

Return a single integer 1 if its exists, 0 otherwise.

## SOLUTION

### APPROACH 1

Sorted form of both string should be same.

```java
public class Solution {

    public int permuteStrings(String A, String B) {

        int n = A.length();
        int m = B.length();

        if(n != m)
            return 0;

        char[] charArray1 = A.toCharArray();
        Arrays.sort(charArray1);

        char[] charArray2 = B.toCharArray();
        Arrays.sort(charArray2);

        for(int i=0; i<n; i++){
            if(charArray1[i] != charArray2[i])
                return 0;
        }

        return 1;

    }

}
```

### APPROACH 2

Frequency of any character from a to z should be same in both strings.

```java
public class Solution {

    public int permuteStrings(String A, String B) {

        int n = A.length();
        int m = B.length();

        if(n != m)
            return 0;

        int[] freq = new int[26];

        for(int i=0; i<n; i++){
            freq[A.charAt(i)-'a']++; // increase freq of char in A
            freq[B.charAt(i)-'a']--; // decrease freq of char in B
        }

        // if freq of all characters are same in A and B
        // the net sum should be 0 for each char from a to z
        // if not, there must be some difference in the chars between them
        // in that case, same permutation cannot be formed
        for(int i=0; i<26; i++){
            if(freq[i] != 0)
                return 0;
        }

        return 1;

    }

}
```
