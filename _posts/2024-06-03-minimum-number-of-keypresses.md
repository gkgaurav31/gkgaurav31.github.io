---
layout: post
title: Minimum Number of Keypresses
date: 2024-06-03 15:05 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode amazon"
categories: "arrays"
---

## Problem Description

You have a keypad with 9 buttons, numbered from 1 to 9, each mapped to lowercase English letters. You can choose which characters each button is matched to as long as:

- All 26 lowercase English letters are mapped to.
- Each character is mapped to by exactly 1 button.
- Each button maps to at most 3 characters.

To type the first character matched to a button, you press the button once. To type the second character, you press the button twice, and so on.

Given a string s, return the minimum number of keypresses needed to type s using your keypad.

Note that the characters mapped to by each button, and the order they are mapped in cannot be changed.

## Solution

In this problem, we do not really need to track which character has the highest frequency. As long as we know the frequencies per character, we can solve it.

```java
class Solution {

    public int minimumKeypresses(String s) {

        // Create an array to store the frequency of each character (a-z).
        // Using Integer array instead of int to use Collections.reverseOrder() later.
        Integer[] arr = new Integer[26];

        // Get the length of the input string
        int n = s.length();

        // Initialize the frequency array with 0
        // To avoid null pointer while doing arr[s.charAt(i) - 'a']++;
        Arrays.fill(arr, 0);

        // Iterate through the string and count the frequency of each character
        for(int i = 0; i < s.length(); i++){
            arr[s.charAt(i) - 'a']++;  // Increment the frequency count for the character
        }

        // Variable to keep track of the total number of characters processed
        int count = 0;
        // Variable to accumulate the minimum keypresses required
        int needed = 0;

        // Sort the frequency array in descending order
        Arrays.sort(arr, Collections.reverseOrder());

        // Iterate through the sorted frequency array
        for(int i = 0; i < 26; i++){
            count++;  // Increment the count of characters considered

            int current = arr[i];  // Get the frequency of the current character

            // Assign keypresses based on the position in the sorted array
            if(count <= 9){
                // First 9 most frequent characters: 1 keypress each
                needed += current;
            } else if(count > 9 && count <= 18){
                // Next 9 most frequent characters: 2 keypresses each
                needed += (current * 2);
            } else {
                // Remaining characters: 3 keypresses each
                needed += (current * 3);
            }
        }

        // Return the total minimum number of keypresses needed
        return needed;
    }
}
```
