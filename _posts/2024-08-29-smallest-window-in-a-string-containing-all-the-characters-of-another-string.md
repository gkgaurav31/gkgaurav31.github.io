---
layout: post
title: Smallest window in a string containing all the characters of another string (geeksforgeeks - SDE Sheet)
date: 2024-08-29 21:07 +0530
author: "Gaurav Kumar"
tags: "java strings sliding_window geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings S and P. Find the smallest window in the string S consisting of all the characters(including duplicates) of the string P. Return "-1" in case there is no such window present. In case there are multiple such windows of same length, return the one with the least starting index.
Note : All characters are in Lowercase alphabets.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/smallest-window-in-a-string-containing-all-the-characters-of-another-string-1587115621/1?page=3)

## SOLUTION

```java
class Solution
{
    public static String smallestWindow(String s, String p)
    {

        int n = s.length();
        int m = p.length();

        // Map to store the frequency of each character in string p
        Map<Character, Integer> countP = new HashMap<>();
        for(int i = 0; i < m; i++)
            countP.put(p.charAt(i), countP.getOrDefault(p.charAt(i), 0) + 1);

        // 'need' represents the total number of unique characters in p that must be present in the window
        int need = countP.size();
        // 'have' represents the count of unique characters from p that are currently met in the window
        int have = 0;

        // Variables to keep track of the minimum length of the window and its starting and ending positions
        int minLength = Integer.MAX_VALUE;
        int resLeft = -1;
        int resRight = -1;

        // Two pointers to represent the current window's left and right boundaries
        int l = 0;
        int r = 0;

        // Map to store the frequency of each character in the current window of string s
        Map<Character, Integer> countS = new HashMap<>();

        while(r < n){

            char ch = s.charAt(r);  // Get the character at the right pointer

            // If the character is not in p, skip it and move the right pointer
            if(!countP.containsKey(ch)){
                r++;
                continue;
            }

            // Otherwise, increment the character's frequency in the current window's map
            countS.put(ch, countS.getOrDefault(ch, 0) + 1);

            // If the frequency of the character in the current window matches the required frequency from p
            // increment 'have' as it means this character's requirement is met
            if(countS.get(ch).equals(countP.get(ch)))
                have++;

            // Try to contract the window from the left if all characters from p are present in the current window
            // The idea is that the current window does have all the characters that we need, but we are looking for a smallest window/substring
            // So, we are trying to reduce the window size from left
            while(have == need){

                // Update the minimum length and window positions if the current window is smaller
                if(r - l + 1 < minLength){
                    minLength = r - l + 1;
                    resLeft = l;
                    resRight = r;
                }

                // Get the character at the left pointer
                char leftChar = s.charAt(l);

                // If the character at the left pointer is in p, decrement its count in the current window map
                if(countP.containsKey(leftChar)){

                    countS.put(leftChar, countS.get(leftChar) - 1);

                    // If the character's frequency in the window is now less than required, decrement 'have'
                    if(countS.get(leftChar) < countP.get(leftChar))
                        have--;

                }

                // Move the left pointer to the right to try and contract the window
                l++;

            }

            // Expand the window by moving the right pointer to the right
            r++;
        }

        // If no valid window was found, return "-1". Otherwise, return the smallest window substring.
        return (minLength == Integer.MAX_VALUE) ? "-1" : s.substring(resLeft, resRight + 1);
    }
}
```
