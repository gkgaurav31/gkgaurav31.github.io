---
layout: post
title: Isomorphic Strings
date: 2023-08-01 20:21 +0530
author: "Gaurav Kumar"
tags: "java hashmap leetcode leetcode150 important"
categories: "hashmap"
---

## PROBLEM DESCRIPTION

Given two strings s and t, determine if they are isomorphic.
Two strings s and t are isomorphic if the characters in s can be replaced to get t.
All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

[leetcode](https://leetcode.com/problems/isomorphic-strings/)

## SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=7yF-U1hLEqQ)

```java
class Solution {
    
    public boolean isIsomorphic(String s, String t) {
        
        // Two HashMaps to store mappings between characters of s and t in both directions.
        // m1 will store the mapping from s to t, and m2 will store the mapping from t to s.
        // IMPORTANT: we need to maintain both sides because as per the question, two character cannot have mapping
        Map<Character, Character> m1 = new HashMap<>();
        Map<Character, Character> m2 = new HashMap<>();

        // Traverse each character in the strings s and t.
        for (int i = 0; i < s.length(); i++) {

            // Get the current characters from both strings.
            Character c1 = s.charAt(i);
            Character c2 = t.charAt(i);

            // Check if there's a mismatch in the mappings between s and t.
            // If there's a mismatch, it means that the characters cannot be isomorphic,
            // so we return false.
            if ((m1.containsKey(c1) && m1.get(c1) != c2) 
                    || (m2.containsKey(c2) && m2.get(c2) != c1)) {
                return false;
            }

            // Update the mappings in both m1 and m2.
            // For the character c1 in s, we map it to character c2 in t in m1.
            // For the character c2 in t, we map it to character c1 in s in m2.
            m1.put(c1, c2);
            m2.put(c2, c1);
        }

        return true;
    }
}
```
