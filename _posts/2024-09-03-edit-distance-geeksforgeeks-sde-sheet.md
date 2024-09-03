---
layout: post
title: Edit Distance (geeksforgeeks - SDE Sheet)
date: 2024-09-03 21:42 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings str1 and str2. Return the minimum number of operations required to convert str1 to str2.
The possible operations are permitted:

- Insert a character at any position of the string.
- Remove any character from the string.
- Replace any character from the string with any other character.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/edit-distance3702/1?page=5)

## SOLUTION

The main logic is this:

```java
// Insert a character in s1
int insert = 1 + ed(s1, s2, i, j - 1, dp);

// Remove a character from s1
int remove = 1 + ed(s1, s2, i - 1, j, dp);

// Replace a character in s1 with a character from s2
int replace = 1 + ed(s1, s2, i - 1, j - 1, dp);

// Choose the operation with the minimum cost and add one for the current operation
dp[i][j] = Math.min(insert, Math.min(remove, replace));
```

```java
class Solution {

    public int editDistance(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        int[][] dp = new int[n][m];

        // Initialize all values in the dp array to -1 to indicate they haven't been computed yet
        for (int i = 0; i < n; i++)
            Arrays.fill(dp[i], -1);

        // Start the recursive function to compute the edit distance from the last character of both strings
        return ed(str1, str2, n - 1, m - 1, dp);
    }

    public int ed(String s1, String s2, int i, int j, int[][] dp) {

        // If both strings are empty, no edits are required
        if (i < 0 && j < 0)
            return 0;

        // If s1 is empty, all characters of s2 need to be inserted into s1
        if (i < 0)
            return j + 1;

        // If s2 is empty, all characters of s1 need to be removed
        if (j < 0)
            return i + 1;

        // If the value is already computed, use it
        if (dp[i][j] == -1) {

            // If the characters are the same, no edit is needed, move to the next characters
            if (s1.charAt(i) == s2.charAt(j)) {
                dp[i][j] = ed(s1, s2, i - 1, j - 1, dp);
            } else {

                // Calculate the cost of each operation: insert, remove, replace

                // Insert a character in s1
                int insert = ed(s1, s2, i, j - 1, dp);

                // Remove a character from s1
                int remove = ed(s1, s2, i - 1, j, dp);

                // Replace a character in s1 with a character from s2
                int replace = ed(s1, s2, i - 1, j - 1, dp);

                // Choose the operation with the minimum cost and add one for the current operation
                dp[i][j] = Math.min(insert, Math.min(remove, replace)) + 1;

            }
        }

        // Return the computed value for this subproblem
        return dp[i][j];

    }
}
```
