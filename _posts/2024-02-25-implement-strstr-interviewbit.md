---
layout: post
title: Implement StrStr (InterviewBit)
date: 2024-02-25 22:50 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Implement strStr().

strstr - locate a substring ( needle ) in a string ( haystack ).

Try not to use standard library string functions for this question.

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

NOTE: String A is haystack, B is needle.

Good clarification questions:

- What should be the return value if the needle is empty?
- What if both haystack and needle are empty?

For the purpose of this problem, assume that the return value should be -1 in both cases.

## SOLUTION

```java
public class Solution {

    public int strStr(final String A, final String B) {

        //A -> haystack
        //B -> needle

        int n = A.length();
        int m = B.length();

        if(n == 0 || m == 0 || m > n){
            return -1;
        }

        // check if we can get the needle if we start from index i of haystack
        for(int i=0; i+m <= n; i++){

            int idx = 0;

            while(idx < m && A.charAt(idx+i) == B.charAt(idx)){
                idx++;
            }

            if(idx == m)
                return i;


        }

        return -1;

    }
}
```
