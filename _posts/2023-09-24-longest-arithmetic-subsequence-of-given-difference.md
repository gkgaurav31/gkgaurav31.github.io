---
layout: post
title: Longest Arithmetic Subsequence of Given Difference
date: 2023-09-24 15:15 +0530
author: "Gaurav Kumar"
tags: "java leetcode leetcodealgo100 dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given an integer array arr and an integer difference, return the length of the longest subsequence in arr which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals difference.
A subsequence is a sequence that can be derived from arr by deleting some or no elements without changing the order of the remaining elements.

[leetcode](https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference/description/)

## Solution

### APPROACH 1 (TLE)

For each element, we check and compare with all its previous element to see if the difference is the required one. If so, update the current length if this has a higher value.

```java
class Solution {

    public int longestSubsequence(int[] arr, int difference) {

        int n = arr.length;

        int[] dp = new int[n];

        for(int i=1; i<n; i++){

            for(int j=0; j<i; j++){

                if(arr[i] - arr[j] == difference){

                    dp[i] = Math.max(dp[i], dp[j]+1);

                }

            }

        }

        int max = 0;

        for(int i=0; i<n; i++){
            max = Math.max(max, dp[i]);
        }

        return max+1;

    }

}
```

### APPROACH 2

We start with an empty "memory" called dp, which is like a dictionary where we'll store information about the longest subsequences.
For each element arr[i] in the array:

- We check if there's a number arr[i] - difference in our dp memory. If it's there, it means we can extend an existing arithmetic subsequence that ends with arr[i] - difference to include arr[i]. We increase the length of the subsequence by 1.

- If arr[i] - difference is not in dp, we consider arr[i] as the start of a new arithmetic subsequence with a length of 1.

While doing this for every element in the array, we keep track of the maximum length of arithmetic subsequences we've found so far. This maximum length represents the longest subsequence that meets the criteria.
We maintain a HashMap for length of longest subsequence ending at that number. Let's say the given differnece is 2. The first number is 1. We check if (1-diff) is present in the hashmap. Since it's not, we add (key=1, value=1) to the hashmap. Let's say that the next number is 3. (3-diff) = (3-2) = 1 is present in the hashmap. So, 3 can be included in that subsequence. The length will be +1 of whatever length the previous number had. So we add (key=3, value=1+1) to the hashmap.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int longestSubsequence(int[] arr, int difference) {

        int n = arr.length;

        // Create a HashMap to store information about subsequences
        Map<Integer, Integer> map = new HashMap<>();

        // Initialize a variable to keep track of the maximum subsequence length
        int max = 1;

        // Loop through the elements of the input array
        for (int i = 0; i < n; i++) {

            // Check if there is a number in the map that is (arr[i] - difference)
            if (map.containsKey(arr[i] - difference)) {

                // If found, extend the current subsequence length by 1 and update the map
                map.put(arr[i], map.get(arr[i] - difference) + 1);

            } else {

                // If not found, start a new subsequence with a length of 1
                map.put(arr[i], 1);

            }

            // Update the maximum subsequence length seen so far
            max = Math.max(max, map.get(arr[i]));

        }

        // Return the maximum subsequence length
        return max;
    }
}
```
