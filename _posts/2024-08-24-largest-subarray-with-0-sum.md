---
layout: post
title: Largest subarray with 0 sum (geeksforgeeks - SDE Sheet)
date: 2024-08-24 19:45 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array having both positive and negative integers. The task is to compute the length of the largest subarray with sum 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1?page=1)

## SOLUTION

```java
class GfG {
    int maxLen(int arr[], int n) {

        // Create a hashmap to store the sum of elements and their corresponding index (storing the cumulative sum)
        Map<Integer, Integer> map = new HashMap<>();

        int sum = 0;
        int maxLength = 0;

        // Traverse the array
        for(int i = 0; i < n; i++) {

            // current sum
            sum = sum + arr[i];

            // Edge case: If the sum up to the current element is 0,
            // it means we found a subarray starting from index 0 to current index with sum 0.
            if(sum == 0) {
                maxLength = i + 1;
                continue;
            }

            // If the sum already exists in the map, it means there is a subarray
            // between the previous index of this sum and the current index with sum 0.
            if(map.containsKey(sum)) {
                // Update maxLength if we find a longer subarray with sum 0
                maxLength = Math.max(maxLength, i - map.get(sum));
            } else {
                // If the sum is not in the map, store it with the current index
                map.put(sum, i);
            }
        }

        return maxLength;
    }
}
```
