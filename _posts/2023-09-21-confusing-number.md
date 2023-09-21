---
layout: post
title: Confusing Number
date: 2023-09-21 20:41 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

A confusing number is a number that when rotated 180 degrees becomes a different number with each digit valid.

We can rotate digits of a number by 180 degrees to form new digits.

- When 0, 1, 6, 8, and 9 are rotated 180 degrees, they become 0, 1, 9, 8, and 6 respectively.
- When 2, 3, 4, 5, and 7 are rotated 180 degrees, they become invalid.

Note that after rotating a number, we can ignore leading zeros.

- For example, after rotating 8000, we have 0008 which is considered as just 8.

Given an integer n, return true if it is a confusing number, or false otherwise.

[leetcode](https://leetcode.com/problems/confusing-number/)

## SOLUTION

```java
class Solution {

    public boolean confusingNumber(int n) {

        // If the input number is 0, it cannot be a confusing number, so return false.
        if (n == 0) return false;

        // Create a variable to store a temporary copy of the input number.
        int temp = n;

        // Create a HashMap to store mappings of digits that become different digits when rotated 180 degrees.
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 0);
        map.put(1, 1);
        map.put(6, 9);
        map.put(8, 8);
        map.put(9, 6);

        // Create a StringBuffer to store the rotated representation of the input number.
        StringBuffer sb = new StringBuffer();

        // Process each digit of the input number.
        while (n != 0) {

            int d = n % 10; // Extract the last digit.
            n = n / 10;     // Remove the last digit.

            // If the digit is 2, 3, 4, 5, or 7, it cannot be rotated 180 degrees, so return false.
            if (d == 2 || d == 3 || d == 4 || d == 5 || d == 7)
                return false;

            // Append the rotated version of the digit to the StringBuffer.
            sb.append(map.get(d) + "");

        }

        // If the rotated representation is the same as the original number, return false (not confusing).
        if (sb.toString().equals(String.valueOf(temp)))
            return false;

        // Otherwise, return true (the number is confusing).
        return true;
    }
}
```
