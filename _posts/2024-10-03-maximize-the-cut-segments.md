---
layout: post
title: Maximize The Cut Segments (geeksforgeeks - SDE Sheet)
date: 2024-10-03 19:37 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an integer n denoting the Length of a line segment. You need to cut the line segment in such a way that the cut length of a line segment each time is either x , y or z. Here x, y, and z are integers.
After performing all the cut operations, your total number of cut segments must be maximum. Return the maximum number of cut segments possible.

Note: if no segment can be cut then return 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/cutted-segments1642/1?page=9)

## SOLUTION

`f(i)` represents the maximum number of cuts for a line segment of length `i`.

The possible lengths to cut the segment into are `x`, `y`, and `z`.

- **Option 1**: `f(i - x) + 1`

  Cut by `x` length. The maximum number of cuts for the remaining length will be `f(i - x)`. Add 1 because we have made one additional cut.

- **Similarly, Option 2**: `f(i - y) + 1`
- **And Option 3**: `f(i - z) + 1`

Since we are looking for the maximum number of cuts, we take the maximum of these three options.

This problem can be solved using a bottom-up approach with a `dp[n + 1]` array.

It is important to handle the "not possible" scenario: for example, when we attempt to cut the segment by `x` length, but the remaining length `i - x` cannot be further divided. We handle this by initializing all values in the `dp` array to `-1`. If `dp[i - x]` is `-1`, it means that it is not possible to divide the segment of length `i - x` using the available lengths `x`, `y`, and `z`.

```java
class Solution
{
    public int maximizeCuts(int n, int x, int y, int z)
    {
        // dp array will store the maximum number of cuts possible for each length from 0 to n
        int[] dp = new int[n+1];

        // IMPORTANT: Initialize the dp array with -1
        // -1 will indicate that it is not possible to divide the length i
        // Take this test case to understand better:
        // n = 7
        // [5, 5, 2]
        Arrays.fill(dp, -1);

        // Base case: If the length of the segment is 0, no cuts are needed, so the number of cuts is 0
        dp[0] = 0;

        // Iterate through each length from 1 to n
        for(int i = 1; i <= n; i++) {

            // Check if it's possible to cut a segment of length `x` from `i`
            // If dp[i-x] is not -1, that means it's possible to cut the remaining length (i-x),
            // so we calculate the new number of cuts and update dp[i] if it results in more cuts
            if(i >= x && dp[i - x] != -1) {
                dp[i] = Math.max(dp[i], dp[i - x] + 1);
            }

            // Similarly, check if it's possible to cut a segment of length `y` from `i`
            if(i >= y && dp[i - y] != -1) {
                dp[i] = Math.max(dp[i], dp[i - y] + 1);
            }

            // Similarly, check if it's possible to cut a segment of length `z` from `i`
            if(i >= z && dp[i - z] != -1) {
                dp[i] = Math.max(dp[i], dp[i - z] + 1);
            }
        }

        // If dp[n] is still -1, it means it's not possible to make any cuts, so return 0
        // Otherwise, return the maximum number of cuts possible for length n
        return dp[n] == -1 ? 0 : dp[n];
    }
}
```
