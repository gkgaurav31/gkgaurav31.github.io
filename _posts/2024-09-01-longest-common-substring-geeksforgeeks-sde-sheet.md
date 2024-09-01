---
layout: post
title: Longest Common Substring (geeksforgeeks - SDE Sheet)
date: 2024-09-01 12:47 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given two strings str1 and str2. Your task is to find the length of the longest common substring among the given strings.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-common-substring1452/1?page=4)

## SOLUTION

### APPROACH 1 - BRUTE FORCE (ACCEPTED)

```java
class Solution {

    public int longestCommonSubstr(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        // maximum length of the common substring
        int maxLength = 0;

        // Iterate through each character in str1
        for(int i = 0; i < n; i++) {

            // Iterate through each character in str2
            for(int j = 0; j < m; j++) {

                // Check if the current characters in str1 and str2 match
                if(str1.charAt(i) == str2.charAt(j)) {

                    // counter to keep track of the length of the current matching substring
                    int c = 0;

                    // two pointers to traverse both strings from the current matching position
                    int l = i;
                    int r = j;

                    // Continue checking characters in both strings while they match and are within bounds
                    while(l < n && r < m && str1.charAt(l) == str2.charAt(r)) {
                        c++;
                        l++;
                        r++;
                    }

                    maxLength = Math.max(maxLength, c);
                }
            }
        }

        return maxLength;
    }
}
```

### APPROACH 2

[takeUForward YouTube](https://www.youtube.com/watch?v=_wP9mWNPL5w)

When we solve the problem of "Longest Common Subsequence", we can consider non-consecutive characters to check if they match. So, this stands true for longest common subsequence:

```plaintext
if(s1[i] == s[j]):
    dp[i][j] == 1 + dp[i-1][j-1];
else
    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
```

In the else part, we are discarding/skipping the element if it did not match. We cannot do that when we talk about substring since it needs to be consecutive. So, we will need to reset the value to 0 if that happens.

```java
class Solution {

    public int longestCommonSubstr(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        // dp[i][j] will contain the length of longest common substring ending with str1[i-1] and str2[j-1]
        int[][] dp = new int[n+1][m+1];

        int max = 0;

        // Iterate over each character of str1
        for(int i = 1; i <= n; i++){

            // Iterate over each character of str2
            for(int j = 1; j <= m; j++){

                // Check if characters at current positions in both strings are equal
                if(str1.charAt(i-1) == str2.charAt(j-1)){

                    // If they match, increment the length of the common substring ending at these characters
                    dp[i][j] = 1 + dp[i-1][j-1];

                    // Update the maximum length found so far
                    max = Math.max(max, dp[i][j]);

                } else {

                    // If they don't match, reset the length of the common substring to 0
                    dp[i][j] = 0;
                }

            }

        }

        return max;

    }

}
```
