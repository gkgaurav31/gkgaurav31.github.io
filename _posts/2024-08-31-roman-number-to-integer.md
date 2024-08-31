---
layout: post
title: Roman Number to Integer (geeksforgeeks - SDE Sheet)
date: 2024-08-31 13:20 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string in roman no format (s) your task is to convert it to an integer . Various symbols and their values are given below.

```plaintext
I 1
V 5
X 10
L 50
C 100
D 500
M 1000
```

[geeksforgeeks](https://www.geeksforgeeks.org/problems/roman-number-to-integer3201/1?page=4)

## SOLUTION

```java
class Solution {

    public int romanToDecimal(String str) {

        int n = str.length();

        // store Roman numerals and their corresponding integer values
        Map<String, Integer> map = new HashMap<>();

        map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);

        int total = 0;

        for (int i = 0; i < n; i++) {

            // If the current character is the last one in the string
            if (i == n - 1) {

                // add its corresponding value
                String current = String.valueOf(str.charAt(i));
                total += map.get(current);

            // If the current character is not the last one
            // Compare it with the value for next character
            } else {

                String current = String.valueOf(str.charAt(i));
                String next = String.valueOf(str.charAt(i + 1));

                // Check if the current numeral is less than the next numeral
                // We need to subtract the current value from total
                if (map.get(current) < map.get(next)) {
                    total -= map.get(current);
                // Otherwise the value can be added
                } else {
                    total += map.get(current);
                }

            }

        }

        return total;

    }

}
```
