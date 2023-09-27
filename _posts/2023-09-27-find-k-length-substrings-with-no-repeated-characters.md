---
layout: post
title: Find K-Length Substrings With No Repeated Characters
date: 2023-09-27 23:33 +0530
author: "Gaurav Kumar"
tags: "java sliding_window leetcode leetcodealgo100"
categories: "sliding_window"
---

## PROBLEM DESCRIPTION

Given a string s and an integer k, return the number of substrings in s of length k with no repeated characters.

[leetcode](https://leetcode.com/problems/find-k-length-substrings-with-no-repeated-characters/)

## SOLUTION

```java
class Solution {

    public int numKLenSubstrNoRepeats(String s, int k) {

        int n = s.length();

        // If k is greater than the length of the string, it's impossible
        // to find substrings of length k with no repeated characters.
        if (k > n) return 0;

        // Create a HashMap to store characters and their counts in the current window.
        Map<Character, Integer> map = new HashMap<>();

        // Initialize the HashMap for the first k characters in the string.
        for (int i = 0; i < k; i++) {
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        }

        // Initialize two pointers i and j to represent the current window.
        int i = 0;
        int j = k - 1;

        // Initialize a count to keep track of valid substrings found.
        int count = 0;

        // Iterate through the string while moving the window.
        while (j < n) {

            // If the size of the HashMap is equal to k, it means we have k unique characters.
            if (map.size() == k) {
                count++;
            }

            // Move the window one character to the right.
            i++;
            j++;

            // Remove the leftmost character from the HashMap.
            Character leftChar = s.charAt(i - 1);
            map.put(leftChar, map.get(leftChar) - 1);

            // If the count of the leftmost character in the HashMap becomes 0,
            // remove it from the HashMap to maintain uniqueness.
            if (map.get(leftChar) == 0) {
                map.remove(leftChar);
            }

            // If j is still within the bounds of the string, add the rightmost
            // character to the HashMap.
            if (j < n) {
                Character rightChar = s.charAt(j);
                map.put(rightChar, map.getOrDefault(rightChar, 0) + 1);
            }
        }

        // Return the count of valid substrings found.
        return count;
    }
}
```
