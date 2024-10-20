---
layout: post
title: Search Pattern (Rabin-Karp Algorithm) (geeksforgeeks - SDE Sheet)
date: 2024-10-17 19:59 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings, one is a text string and the other is a pattern string. The task is to print the starting indexes of all the occurrences of the pattern string in the text string. For printing, the Starting Index of a string should be taken as 1. The strings will only contain lowercase English alphabets ('a' to 'z').

[geeksforgeeks](https://www.geeksforgeeks.org/problems/search-pattern-rabin-karp-algorithm--141631/1?page=5)

## SOLUTION

### INITIAL APPROACH (ACCEPTED)

The code searches for all occurrences of a pattern in a given text using a rolling hash technique.

It first calculates hash values for both the pattern and the initial window of text. If their hashes match, it compares the actual strings to confirm a match. Then, it slides over the text one character at a time, updating the hash efficiently by removing the previous character's contribution and adding the new one. For each match, it stores the 1-based starting index in a list. The use of modulo ensures hash values stay manageable, avoiding overflow.

Example:

First, we convert the pattern into a hash number by adding up the ASCII values (character codes) of its characters.
For example, if the pattern is "abc", the hash could be something like this:

'a' = 97, 'b' = 98, 'c' = 99.

So, the pattern hash would be: 97 + 98 + 99 = 294.

If the text is "abcabcabc" and the pattern is "abc", the first 3 characters ("abc") have the same hash: 294.

If the hash match, it's a potential match. We compare the current window with the pattern to confirm.

```java
import java.util.ArrayList;

class Solution {

    // A large prime number to use for the modulo operation in the hash function
    private static final int M = 100000009;

    ArrayList<Integer> search(String pattern, String text) {

        int p = pattern.length();
        int t = text.length();

        // List to store the 1-indexed starting positions of matches
        ArrayList<Integer> list = new ArrayList<>();

        // If the pattern is longer than the text, no matches are possible
        if (p > t) {
            return list;
        }

        // Initialize hash values for both the pattern and the current window of text
        int textHash = 0;
        int patternHash = 0;

        // Compute the hash values for the pattern and the first window of text
        // We are using a very basic hash function which is just summing up the char values of the window
        // This is not an ideal hash function and will lead to many conflicts. We will use it for understanding purpose for now. The code will still work
        for (int i = 0; i < p; i++) {

            // Hash for pattern
            patternHash = (patternHash % M + pattern.charAt(i) % M) % M;

            // Hash for first window of text
            textHash = (textHash % M + text.charAt(i) % M) % M;

        }

        // Check if the first window matches the pattern
        if (textHash == patternHash && pattern.equals(text.substring(0, p))) {
            list.add(1);
        }

        // Slide over the text one character at a time
        for (int i = 1; i <= t - p; i++) {

            // Update the hash value by removing the hash of the previous window's first character
            // and adding the hash of the new character in the current window
            textHash = (textHash % M - text.charAt(i - 1) % M + text.charAt(i + p - 1) % M + M) % M;

            // Check if the current window matches the pattern
            if (textHash == patternHash && pattern.equals(text.substring(i, i + p))) {
                list.add(i + 1);
            }
        }

        return list;
    }
}
```

### IMPROVING THE HASH

The current rolling hash calculation is very basic and could lead to hash collisions more often than necessary.

A more common approach is to use a base (e.g., 256) in the hash function. This reduces the chance of two different strings having the same hash (collision).
