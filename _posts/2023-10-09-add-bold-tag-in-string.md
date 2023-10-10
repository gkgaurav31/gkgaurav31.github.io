---
layout: post
title: Add Bold Tag in String
date: 2023-10-09 20:08 +0530
author: "Gaurav Kumar"
tags: "java intervals strings leetcode leetcodealgo100"
categories: "strings"
---

## PROBLEM DESCRIPTION

You are given a string s and an array of strings words.

You should add a closed pair of bold tag `<b>` and `</b>` to wrap the substrings in s that exist in words.

- If two such substrings overlap, you should wrap them together with only one pair of closed bold-tag.
- If two substrings wrapped by bold tags are consecutive, you should combine them.

Return s after adding the bold tags.

[leetcode](https://leetcode.com/problems/missing-ranges/)

## SOLUTION

```java
class Solution {

    public String addBoldTag(String s, String[] words) {

        // Create a boolean array 'bold' to mark which characters in 's' should be bolded.
        boolean[] bold = new boolean[s.length()];

        // Iterate through the array of words to find and mark their occurrences in 's'.
        for (int i = 0; i < words.length; i++) {

            String word = words[i];

            // Find the first occurrence of 'word' in 's'.
            int idx = s.indexOf(word);

            // mark all positions corresponding to the current word in string s as bold
            while (idx != -1) {
                for (int j = idx; j <= idx + word.length() - 1; j++) {
                    bold[j] = true;
                }

                // Find the next occurrence of 'word' in 's'.
                idx = s.indexOf(word, idx + 1);
            }
        }

        String openTag = "<b>";
        String closeTag = "</b>";

        // answer string
        StringBuffer sb = new StringBuffer();

        // go through each position to check if it should be bold
        for (int i = 0; i < bold.length; i++) {

            if (bold[i] == true) {

                // If the current character should be bolded and it's the start of a new bolded sequence.
                if (i == 0 || bold[i - 1] == false) {
                    sb.append(openTag);
                }

            }

            // Append the current character to the result.
            sb.append(s.charAt(i));

            if (bold[i] == true) {

                // If the current character should be bolded and it's the end of a bolded sequence.
                if (i == bold.length - 1 || bold[i + 1] == false) {
                    sb.append(closeTag);
                }

            }
        }

        return sb.toString();
    }
}
```
