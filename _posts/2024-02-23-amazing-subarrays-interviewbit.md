---
layout: post
title: Amazing Subarrays (InterviewBit)
date: 2024-02-23 19:58 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a string `A`, and you have to find all the amazing substrings of `A`.
An amazing Substring is one that starts with a vowel `(a, e, i, o, u, A, E, I, O, U)`.

Note: Take the mod of the answer with `10003`.

## SOLUTION

```java
public class Solution {

    int M = 10003;

    public int solve(String A) {

        int total = 0;
        int n = A.length();

        Set<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));

        for(int i=0; i<n; i++){

            char ch = A.charAt(i);

            if(vowels.contains(ch)){

                // if char at index i is a vowel
                // there can be more (n-i) substring which can start from i
                total = (total%M + (n-i)%M)%M;

            }

        }

        return total;

    }

}
```
