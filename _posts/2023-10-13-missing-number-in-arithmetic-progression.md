---
layout: post
title: Missing Number In Arithmetic Progression
date: 2023-10-13 20:31 +0530
author: "Gaurav Kumar"
tags: "java binary_search arrays leetcode leetcodealgo100"
categories: "binary_search"
---

## PROBLEM DESCRIPTION

In some array arr, the values were in arithmetic progression: the values arr[i + 1] - arr[i] are all equal for every 0 <= i < arr.length - 1.
A value from arr was removed that was not the first or last value in the array.
Given arr, return the removed value.

[leetcode](https://leetcode.com/problems/missing-number-in-arithmetic-progression/)

## SOLUTION

```java
class Solution {

    public int missingNumber(int[] arr) {

        int n = arr.length;

        // Calculate the common difference (r) of the arithmetic progression.
        // As per the problem, the first and last numbers cannot be incorrect, so we can use that to find r
        int r = (arr[n-1] - arr[0]) / n;

        // Iterate through the array to find the missing value
        for (int i = 1; i < n; i++) {
            // the next element in the AP should be: previous element + common difference
            if (arr[i-1] + r != arr[i]) {
                // If it's not, return the expected value in the progression
                return arr[i-1] + r;
            }
        }

        return arr[0]; // r=0, case: [x, x, x, x] -> all same elements

    }

}
```
