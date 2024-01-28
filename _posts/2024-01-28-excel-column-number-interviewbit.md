---
layout: post
title: Excel Column Number (InterviewBit)
date: 2024-01-28 22:39 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a column title A as appears in an Excel sheet, return its corresponding column number.

## SOLUTION

```java
public class Solution {

    public int titleToNumber(String A) {

        int res = 0;
        int n = A.length();

        // Iterate through each character in reverse order
        for(int i=n-1; i>=0; i--){

            // Extract the current character
            char ch = A.charAt(i);

            // Calculate the numeric value of the character (A=1, B=2, C=3 etc.)
            int chVal = ch - 'A' + 1;

            // Calculate the power of 26 based on the position of the character in the title
            int pow = n-i-1;

            // Update the result by adding the product of character value and 26 raised to the power
            res = res + chVal * (int) Math.pow(26, pow);

        }

        return res;

    }

}
```
