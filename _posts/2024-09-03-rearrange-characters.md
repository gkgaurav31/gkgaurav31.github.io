---
layout: post
title: Rearrange characters (geeksforgeeks - SDE Sheet)
date: 2024-09-03 00:01 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S with repeated characters. The task is to rearrange characters in a string such that no two adjacent characters are the same.
Note: The string has only lowercase English alphabets and it can have multiple solutions. Return any one of them.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rearrange-characters4649/1?page=5)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=wZENBuWh-C0)

```java
class Solution {

    public static String rearrangeCharacters(String str) {

        int n = str.length();

        // Convert the string to a character array
        char[] arr = str.toCharArray();

        // Frequency array for each character ('a' to 'z')
        int[] freq = new int[26];

        // To keep track of the maximum frequency of any character
        int maxFrequency = 0;

        // To store the character with the maximum frequency
        char maxFrequencyChar = '#';

        // Compute the frequency of each character and find the character with the maximum frequency
        for (int i = 0; i < n; i++) {

            freq[arr[i] - 'a']++;

            if (freq[arr[i] - 'a'] > maxFrequency) {
                maxFrequency = freq[arr[i] - 'a'];
                maxFrequencyChar = arr[i];
            }

        }

        // Check if it is possible to rearrange the characters such that no two adjacent characters are the same
        // If there is some character which has a frequency of more than half of the length of the string, then it's not possible.
        if (maxFrequency > (n + 1) / 2) {
            return "-1"; // Not possible to rearrange
        }

        char[] res = new char[n];

        // Index to place characters in the result array
        int idx = 0;

        // Place the most frequent character at every alternate position
        while (freq[maxFrequencyChar - 'a'] > 0) {

            freq[maxFrequencyChar - 'a']--;
            res[idx] = maxFrequencyChar;
            idx += 2; // Move to the next alternate position, because adjacent cannot be same

        }

        // Place the remaining characters in the result array
        for (int i = 0; i < 26; i++) {

            while (freq[i] > 0) {

                // If we have reached the end of the array, start placing characters at odd indices
                if (idx >= n) {
                    idx = 1;
                }

                // Decrease the frequency of the character
                freq[i]--;

                // Place the character at the current index
                res[idx] = (char) (i + 'a');

                // Move to the next alternate position
                idx += 2;

            }

        }

        return String.valueOf(res);
    }
}
```
