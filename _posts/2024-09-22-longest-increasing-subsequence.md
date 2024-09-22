---
layout: post
title: Longest Increasing Subsequence (geeksforgeeks - SDE Sheet)
date: 2024-09-22 00:29 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array a[ ] of n integers, find the Length of the Longest Strictly Increasing Subsequence.

A sequence of numbers is called "strictly increasing" when each term in the sequence is smaller than the term that comes after it.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-increasing-subsequence-1587115620/1?page=8)

## SOLUTION

### APPROACH 1 (DYNAMIC PROGRRAMMING) - TLE

- The dp array is used to store the length of the longest subsequence ending at each index.
- Initially, each element is considered a subsequence of length 1 (hence Arrays.fill(dp, 1)).
- For each element a[i], the inner loop checks all previous elements a[j] where j < i. If a[i] > a[j], it means we can append a[i] to the subsequence ending at a[j], and we update dp[i] accordingly.
- Finally, you track the maximum length of any subsequence using the variable max.

```java
class Solution {

    static int longestSubsequence(int n, int a[]) {

        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        dp[0] = 1;

        int max = 1;

        for(int i=1; i<n; i++){

            for(int j=0; j<i; j++){

                if(a[i] > a[j])
                    dp[i] = Math.max(dp[i], dp[j] + 1);

                max = Math.max(max, dp[i]);
            }
        }

        return max;

    }

}
```

### APPROACH 2

[USING BINARY SEARCH](https://www.youtube.com/watch?v=on2hvxBXJH4)

We can solve this by using a list to keep track of the smallest possible ending elements for increasing subsequences of different lengths. For each element in the array, we use binary search to find where it should be placed in the list. If it's larger than all elements in the list, we add it to the end. Otherwise, we replace the appropriate element to maintain the smallest possible values. This way, the list always represents the smallest tails of increasing subsequences, and its size gives the length of the longest increasing subsequence.

```java
class Solution {

    // Method to find the length of the longest increasing subsequence
    static int longestSubsequence(int n, int a[]) {

        List<Integer> lis = new ArrayList<>();

        // Add the first element of the array to the list, as the starting point
        lis.add(a[0]);

        // Traverse the remaining elements of the array
        for(int i = 1; i < n; i++) {

            // Current element in the array
            int current = a[i];

            // Use binary search to find the position where the current element
            // could replace an element in the 'lis' list. The method returns
            // the index if the element exists, otherwise, it returns a negative
            // value indicating the insertion point.
            int idx = Collections.binarySearch(lis, current);

            // If the element is not found in 'lis' (i.e., idx is negative)
            if (idx < 0) {

                // Calculate the insertion point for the current element
                // If key is not present, the it returns "(-(insertion point) - 1)"
                int insertionPoint = -idx - 1;

                // If the insertion point is at the end of the list, it means 'current' is larger than any element in 'lis', so it extends the LIS.
                if (insertionPoint == lis.size()) {

                    lis.add(current);

                } else {

                    // Otherwise, replace the element at the insertion point with 'current'
                    lis.set(insertionPoint, current);

                }
            }
        }

        return lis.size();
    }
}
```
