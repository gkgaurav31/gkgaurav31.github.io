---
layout: post
title: Shortest Way to Form String
date: 2023-09-25 19:54 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

Given two strings source and target, return the minimum number of subsequences of source such that their concatenation equals target. If the task is impossible, return -1.

> 1 <= source.length, target.length <= 1000

```text
Input: source = "xyz", target = "xzyxz"
Output: 3
Explanation: The target string can be constructed as follows "xz" + "y" + "xz".
```

[leetcode](https://leetcode.com/problems/shortest-way-to-form-string/)

## SOLUTION

```java
class Solution {

    public int shortestWay(String source, String target) {

        int count = 0; // Initialize a variable to keep track of the number of subsequences.

        int n = source.length(); // Get the length of the source string.
        int m = target.length(); // Get the length of the target string.

        int i = 0; // Initialize a pointer for the source string.
        int j = 0; // Initialize a pointer for the target string.

        // Start iterating through the target string until we reach its end.
        while (j < m) {

            boolean found = false; // Initialize a flag to check if at least one character has matched

            // Iterate through the source string while comparing it to the target.
            while (i < n && j < m) { // We need to check j<m as well since the target string can be smaller

                // If the current characters match, move both pointers and set the 'found' flag to true.
                if (source.charAt(i) == target.charAt(j)) {
                    i++;
                    j++;
                    found = true;
                } else {
                    i++; // If characters don't match, increment the source pointer. (since we need any valid subsequence)
                }
            }

            // If we found at least one match also in the above step, we have one more subsequence to include in our answer
            if (found) {
                count++;
                i = 0;
            } else {
                return -1; // If no match was found in the inner loop, it's impossible to form the target from the source, so return -1.
            }
        }

        return count; // Return the total count of subsequences needed to form the target.
    }
}
```
