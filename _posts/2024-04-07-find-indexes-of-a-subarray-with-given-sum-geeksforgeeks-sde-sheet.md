---
layout: post
title: Find Indexes of a subarray with given sum (geeksforgeeks - SDE Sheet)
date: 2024-04-07 12:22 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an unsorted array `A` of size `N` that contains only non negative integers, find a continuous sub-array that adds to a given number `S` and return the left and right index(1-based indexing) of that subarray.

In case of multiple subarrays, return the subarray indexes which come first on moving from left to right.

**Note:**- You have to return an ArrayList consisting of two elements left and right. In case no such subarray exists return an array consisting of element `-1`.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/subarray-with-given-sum-1587115621/1)

## SOLUTION

This can be solved using sliding windows technique, however, it has some small nuance and we need to maintain left and right pointers of the window carefully.

Important edge case:

- If the sum we are looking for is `0`, we would get that sum if `l` and `r` are equal. So, we should add a logic to ensure that `l<r` while interating the array.

```java
class Solution {

    static ArrayList<Integer> subarraySum(int[] arr, int n, int s) {

        int sum = 0; // Tracks the current sum of elements in the window.

        int l = 0;   // Left index of the window.
        int r = 0;   // Right index of the window.

        ArrayList<Integer> res = new ArrayList<>();

        // Loop through the array until the right index reaches the end.
        while (r < n) {

            // Add the current element to the sum.
            sum += arr[r];

            // Check if the sum exceeds the given target 's'.
            while (l < r && sum > s) {

                // reduce the window from left side (to decrease the sum)
                sum -= arr[l];
                l++;

            }

            // If the sum equals the target 's', we've found the subarray.
            // we need to return 1-based indexing as the answer
            if (sum == s) {

                res.add(l + 1);
                res.add(r + 1);
                return res;
            }

            // Increment the right index to expand the window.
            r++;
        }

        // no sub array with sum s was found
        res.add(-1);
        return res;
    }
}
```
