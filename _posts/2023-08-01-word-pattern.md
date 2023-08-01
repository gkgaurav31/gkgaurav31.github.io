---
layout: post
title: Word Pattern
date: 2023-08-01 20:44 +0530
author: "Gaurav Kumar"
tags: "java hashmap leetcode leetcode150"
categories: "hashmap"
---

## PROBLEM DESCRIPTION

Given a pattern and a string s, find if s follows the same pattern.
Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in s.

[leetcode](https://leetcode.com/problems/word-pattern/)

## SOLUTION

This problem is similar to [Isomorphic Strings]({% post_url 2023-08-01-isomorphic-strings %})

```java
class Solution {
    
    public boolean wordPattern(String pattern, String s) {
        
        // Two HashMaps to store the mappings between characters in pattern and words in s,
        // in both directions (pattern to word and word to pattern).
        Map<Character, String> psMap = new HashMap<>(); // pattern-to-word mapping
        Map<String, Character> spMap = new HashMap<>(); // word-to-pattern mapping
        
        // Split the input string s into individual words based on spaces.
        String[] words = s.split(" ");

        // If the number of words in s does not match the length of the pattern,
        // then s cannot follow the same pattern, and we return false.
        if (words.length != pattern.length())
            return false;

        // Iterate through each character in pattern and its corresponding word in s.
        for (int i = 0; i < words.length; i++) {
            Character p = pattern.charAt(i);
            String word = words[i];

            // Check if the pattern-to-word mapping is consistent.
            // If the character p is already mapped to a different word than the current word,
            // then s does not follow the same pattern, and we return false.
            if (psMap.containsKey(p) && !psMap.get(p).equals(word))
                return false;

            // Check if the word-to-pattern mapping is consistent.
            // If the word is already mapped to a different character than the current character p,
            // then s does not follow the same pattern, and we return false.
            if (spMap.containsKey(word) && spMap.get(word) != p)
                return false;

            // Update the mappings in both psMap and spMap.
            psMap.put(p, word); // Map character p to the current word.
            spMap.put(word, p); // Map the current word to character p.
        }

        return true;
    }
}

```
