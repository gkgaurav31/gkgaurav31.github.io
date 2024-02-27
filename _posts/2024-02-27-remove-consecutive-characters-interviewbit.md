---
layout: post
title: Remove Consecutive Characters (InterviewBit)
date: 2024-02-27 21:39 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a string A and integer B, remove all consecutive same characters that have length `exactly` B.

NOTE : All the consecutive same characters that have length exactly B will be removed simultaneously.

## SOLUTION

```java
public class Solution {

    public String solve(String A, int B) {

        int n = A.length();

        // Counter to keep track of consecutive characters
        int currentCount = 0;

        // Previous character initialized with a non-alphanumeric character
        char prev = '#';

        // StringBuffer to store the result
        StringBuffer res = new StringBuffer();

        // StringBuffer to store consecutive characters temporarily
        StringBuffer temp = new StringBuffer();

        for(int i = 0; i < n; i++){

            // Current character in the iteration
            char ch = A.charAt(i);

            // If the current character is the same as the previous character
            if(ch == prev){

                // Append the character to the temporary StringBuffer
                temp.append(ch + "");

                // Increase the count of consecutive characters
                currentCount++;

            }else{  // If the current character is different from the previous character

                // If there were consecutive characters and their count is not exactly B
                if(currentCount != B){
                    // Append the consecutive characters stored in temp to the result
                    res.append(temp);
                }

                // Reset the temporary StringBuffer with the current character
                temp = new StringBuffer(ch + "");

                // Update the previous character to the current character
                prev = ch;

                // Reset the count of consecutive characters to 1
                currentCount = 1;
            }

        }

        // If there were consecutive characters at the end and their count is not exactly B
        if(currentCount > 0 && currentCount != B){
            // Append the consecutive characters stored in temp to the result
            res.append(temp);
        }

        // Convert the StringBuffer result to a String and return
        return res.toString();
    }
}
```
