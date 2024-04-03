---
layout: post
title: Length of Last Word (InterviewBit)
date: 2024-02-16 22:41 +0530
author: "Gaurav Kumar"
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

Please make sure you try to solve this problem without using library functions. Make sure you only traverse the string once.

## SOLUTION

```java
public class Solution {

    public int lengthOfLastWord(final String A) {

        int length = 0;

        int i = A.length()-1;

        while(i>=0 && A.charAt(i) == ' '){
            i--;
        }

        while(i>=0 && A.charAt(i) != ' '){
            length++;
            i--;
        }

        return length;

    }

}
```
