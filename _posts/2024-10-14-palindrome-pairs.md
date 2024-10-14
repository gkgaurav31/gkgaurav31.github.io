---
layout: post
title: Palindrome Pairs (geeksforgeeks - SDE Sheet)
date: 2024-10-14 21:15 +0530
author: "Gaurav Kumar"
tags: "java two_pointers geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of strings arr[] of size N, find if there exists 2 strings arr[i] and arr[j] such that arr[i]+arr[j] is a palindrome i.e the concatenation of string arr[i] and arr[j] results into a palindrome.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/palindrome-pairs/1?page=10)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=tSHSwRkznDk)

> [str1] + [str2] = [str3]

We want to find two strings, `str1` and `str2`, in the array such that their concatenation forms a palindrome, `str3`.

Let's say `str1` is "abcba". We don't know which part of this string will form the left side of the palindrome, so we need to consider all possible left substrings. The first left substring is "a". Is it a palindrome? Yes. Now, we reverse the remaining part, which is "abcb". If "abcb" is present in the array, we can form a valid palindrome by concatenating these two strings.

Next, we check the left substring "ab", but it's not a palindrome, so we move forward.

We need to perform the same checks for substrings starting from the right side as well.

```java
class Solution {

    static boolean palindromepair(int n, String arr[]) {

        // Map to store strings and their respective indices
        Map<String, Integer> map = new HashMap<>();

        // Flag to track if an empty string is present in the array
        boolean isEmptyPresent = false;

        for (int i = 0; i < n; i++) {
            map.put(arr[i], i);

            // Check if the current string is empty
            if (arr[i].equals("")) {
                isEmptyPresent = true;
            }
        }

        // Handle the edge case where an empty string is present
        if (isEmptyPresent) {

            // Check if any string in the array forms a palindrome by itself
            for (int i = 0; i < n; i++) {

                // Skip the empty string
                if (!arr[i].equals("")) {
                    // If a palindrome string exists, return true
                    if (isPalindrome(arr[i])) {
                        return true;
                    }
                }
            }
        }

        // Loop through each string in the array
        for (int i = 0; i < n; i++) {

            String s = arr[i];

            // Loop through each character in the string to form substrings
            for (int j = 0; j < s.length(); j++) {

                // Split the string into two parts: left and right
                String leftStr = s.substring(0, j + 1);
                String rightStr = s.substring(j + 1);

                // Reverse both substrings
                String leftStrReverse = reverseString(leftStr);
                String rightStrReverse = reverseString(rightStr);

                // Check if the left part is a palindrome and the reverse of the right part exists in the map
                if (isPalindrome(leftStr) && map.containsKey(rightStrReverse) && map.get(rightStrReverse) != i) {
                    return true;
                }

                // Check if the right part is a palindrome and the reverse of the left part exists in the map
                if (isPalindrome(rightStr) && map.containsKey(leftStrReverse) && map.get(leftStrReverse) != i) {
                    return true;
                }
            }
        }

        // Return false if no palindrome pair is found
        return false;
    }

    static String reverseString(String s) {
        StringBuilder reversed = new StringBuilder(s);
        return reversed.reverse().toString();
    }

    static boolean isPalindrome(String s) {

        int i = 0;
        int j = s.length() - 1;

        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }

        return true;
    }
}
```
