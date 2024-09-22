---
layout: post
title: Choose and Swap (geeksforgeeks - SDE Sheet)
date: 2024-09-21 19:58 +0530
author: "Gaurav Kumar"
tags: "java greedy geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a string str of lower case english alphabets. You can choose any two characters in the string and replace all the occurences of the first character with the second character and replace all the occurences of the second character with the first character. Your aim is to find the lexicographically smallest string that can be obtained by doing this operation at most once.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/choose-and-swap0531/1?page=8)

## SOLUTION

### BRUTE FORCE (ACCEPTED)

Try swapping every pair possible from the given string. If it's lexographically smaller than given str, it's a possible candidate. Maintain a result string and update it if the current string formed is smaller.

```java
class Solution {

    String chooseandswap(String str) {

        // Initialize the result with the original string
        String res = str;

        // Create a set of unique characters from the input string
        Set<Character> set = new HashSet<>();
        for(int i = 0; i < str.length(); i++)
            set.add(str.charAt(i));

        // Convert the set to a list of characters to allow indexed access
        List<Character> list = new ArrayList<>(set);

        // Loop through all pairs of characters in the list to attempt swapping
        for (int i = 0; i < list.size(); i++) {

            for (int j = i + 1; j < list.size(); j++) {

                // Get two characters to swap: 'a' from list.get(i) and 'b' from list.get(j)
                char a = list.get(i);
                char b = list.get(j);

                // Create a copy of the original string as a character array for swapping
                char[] arr = str.toCharArray();

                // Swap all occurrences of 'a' with 'b' and 'b' with 'a' in the character array
                for (int k = 0; k < arr.length; k++) {
                    if (arr[k] == a)
                        arr[k] = b;
                    else if (arr[k] == b)
                        arr[k] = a;
                }

                // Convert the modified character array back to a string
                String newStr = String.valueOf(arr);

                // If the new string is lexicographically smaller, update the result
                if (newStr.compareTo(res) < 0)
                    res = newStr;
            }
        }

        return res;
    }

}
```
