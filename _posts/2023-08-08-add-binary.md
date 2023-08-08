---
layout: post
title: Add Binary
date: 2023-08-08 22:43 +0530
author: "Gaurav Kumar"
tags: "java  bit_manipulation leetcode leetcode150"
categories: "bit_manipulation"
---
## PROBLEM DESCRIPTION

Given two binary strings a and b, return their sum as a binary string.

[leetcode](https://leetcode.com/problems/add-binary/)

## SOLUTION

```java
class Solution {
    
    public String addBinary(String a, String b) {

        // Initialize indices to traverse the input strings from right to left
        int i = a.length() - 1;
        int j = b.length() - 1;

        // Initialize a variable to keep track of the carry during addition
        int carry = 0;

        // Initialize a StringBuffer to store the resulting binary sum
        StringBuffer sb = new StringBuffer();

        // Iterate through the strings and the carry until both strings and carry are exhausted
        while (i >= 0 || j >= 0 || carry != 0) {

            // Extract the binary digits at the current positions, if available
            int x = i >= 0 ? a.charAt(i) - '0' : 0;
            int y = j >= 0 ? b.charAt(j) - '0' : 0;

            // Calculate the sum of the digits along with the carry
            int sum = x + y + carry;

            // Insert the least significant bit of the sum at the beginning of the result
            sb.insert(0, String.valueOf(sum % 2));

            // Update the carry for the next iteration
            carry = sum / 2;
                
            // Move to the next positions in both strings
            i--;
            j--;
        }

        return sb.toString();
    }
}
```
