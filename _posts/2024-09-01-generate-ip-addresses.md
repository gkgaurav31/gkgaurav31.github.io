---
layout: post
title: Generate IP Addresses (geeksforgeeks - SDE Sheet)
date: 2024-09-01 23:31 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S containing only digits, Your task is to complete the function genIp() which returns a vector containing all possible combinations of valid IPv4 IP addresses and takes only a string S as its only argument.
Note: Order doesn't matter. A valid IP address must be in the form of A.B.C.D, where A, B, C, and D are numbers from 0-255. The numbers cannot be 0 prefixed unless they are 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/generate-ip-addresses/1?page=5)

## SOLUTION

The IP address is of the form:

> **\_** . **\_** . **\_** . **\_**

Each part can range between [0, 255]. This means, each part can have at most 3 digits.

We try to form the first part using initial one, two and three digits. Then, we recursively do the same thing for other parts. Once we have 4 parts formed, we add it to the answer list.

We can have a number like 345, which is > 255. So, we can create a method to verify if the current number that the digits form is valid or not.

Also, a number which starts with 0, like "05" is invalid. Remember, 0 itself is valid since the range is [0, 255].

```java
import java.util.ArrayList;
import java.util.List;

class Solution {

    public ArrayList<String> genIp(String s) {

        int n = s.length();

        // If the string length is greater than 12, it's impossible to form a valid IP address.
        if (n > 12)
            return new ArrayList<>(List.of("-1"));

        return genIpHelper(s, 0, new ArrayList<>(), new ArrayList<>());
    }

    public ArrayList<String> genIpHelper(String s, int idx, ArrayList<String> res, ArrayList<String> current) {
        // Base case: If we have used all characters in the string and have exactly 4 segments,
        // join them with '.' and add to the result list.
        if (idx == s.length() && current.size() == 4) {
            res.add(String.join(".", current));
            return res;
        }

        // If we already have 4 segments and haven't used all characters, stop further processing.
        if (current.size() == 4)
            return res;

        // Try to form each part of the IP address with at most 3 digits
        for (int i = idx; i <= idx + 2 && i < s.length(); i++) {

            // Extract the substring to form the next part of the IP address
            String next = s.substring(idx, i + 1);

            // If the extracted part is not valid, break the loop as no further valid IP segments can be formed
            if (!isValid(next))
                break;

            // Add the valid segment to the current IP address
            current.add(next);

            // Recursively call the helper to add the next valid segment
            genIpHelper(s, i + 1, res, current);

            // Backtrack: remove the last added segment to try the next possibility
            current.remove(current.size() - 1);

        }

        return res;

    }

    public boolean isValid(String s) {

        // Segment length should be between 1 and 3
        if (s.length() == 0 || s.length() > 3) return false;

        // Check if the segment starts with '0' and has more than one character
        // (invalid cases like "01", "001", etc.)
        if (s.length() > 1 && s.charAt(0) == '0') return false;

        // Parse the integer and check if it's within the valid range (0 to 255)
        int num = Integer.parseInt(s);
        return num >= 0 && num <= 255;

    }
}
```
