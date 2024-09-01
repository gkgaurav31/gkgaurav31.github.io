---
layout: post
title: Implement Atoi
date: 2024-09-01 13:38 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string s, the objective is to convert it into integer format without utilizing any built-in functions. If the conversion is not feasible, the function should return -1.

Note: Conversion is feasible only if all characters in the string are numeric or if its first character is '-' and rest are numeric.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/implement-atoi/1?page=4)

## SOLUTION

### APPROACH 1

```java
class Solution {

    public int atoi(String s) {

        int n = s.length();

        // Determine if the number is positive or negative
        // If it has an invalid value, that will be checked in the for loop later
        boolean isPositive = s.charAt(0) == '-' ? false : true;

        // Initialize a variable to store the final integer value
        int total = 0;
        // Initialize a position variable to calculate place value (units, tens, hundreds, etc.)
        int pos = 0;

        // Loop through the string from the end to the beginning
        for(int i = n - 1; i >= 0; i--) {

            // Get the current character from the string
            char ch = s.charAt(i);

            // If the number is negative and we're at the first character, skip this iteration
            if (!isPositive && i == 0)
                continue;

            // Check if the character is a digit; if not, return -1 as conversion is not feasible
            if (ch < '0' || ch > '9')
                return -1;

            // Convert the character to its corresponding integer value and add to the total
            // The expression calculates the place value (e.g., units, tens, hundreds) using Math.pow(10, pos)
            total += ((Integer.valueOf(String.valueOf(ch))) * Math.pow(10, pos++));

        }

        // If the number was determined to be negative, negate the total
        if (!isPositive)
            total = -total;

        return total;

    }

}
```
