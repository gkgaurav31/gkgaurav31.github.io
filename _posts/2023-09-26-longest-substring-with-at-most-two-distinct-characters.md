---
layout: post
title: Longest Substring with At Most Two Distinct Characters
date: 2023-09-26 19:22 +0530
author: "Gaurav Kumar"
tags: "java sliding_window leetcode leetcodealgo100"
categories: "sliding_window"
---

## PROBLEM DESCRIPTION

Given a string s, return the length of the longest substring that contains at most two distinct characters.

[leetcode](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)

## SOLUTION

```java
class Solution {

    public int lengthOfLongestSubstringTwoDistinct(String s) {

        // Get the length of the input string
        int n = s.length();

        // Create a map to store characters and their counts
        Map<Character, Integer> map = new HashMap<>();

        // Initialize two pointers for the sliding window
        int i=0; // Left pointer -> start of sliding window
        int j=0; // Right pointer -> end of sliding window

        // Initialize the maximum length of the substring with two distinct characters
        int maxLength = 1;

        // Start iterating through the string
        while(j < n){

            // Get the character at the right pointer
            Character c = s.charAt(j);

            // Update the character count in the map
            map.put(c, map.getOrDefault(c, 0) + 1);

            // Check if the number of distinct characters in the map is less than or equal to 2
            if(map.size() <= 2){
                // Update the maximum length if the current substring has at most two distinct characters
                maxLength = Math.max(maxLength, j - i + 1);
            } else {
                // If there are more than two distinct characters in the map, move the left pointer (reduce windows size)
                // and reduce the counts of characters until we have at most two distinct characters again.
                while(map.size() > 2){

                    // Get the character at the left pointer
                    Character leftChar = s.charAt(i);

                    // Decrease the count of the character
                    map.put(leftChar, map.get(leftChar) - 1);

                    // If the count reaches 0, remove the character from the map
                    if(map.get(leftChar) == 0)
                        map.remove(leftChar);

                    // Move the left pointer to the right
                    i++;

                }

            }

            // Move the right pointer to the right
            j++;

        }

        return maxLength;

    }

}
```
