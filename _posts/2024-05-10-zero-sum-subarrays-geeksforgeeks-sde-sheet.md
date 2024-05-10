---
layout: post
title: Zero Sum Subarrays (geeksforgeeks - SDE Sheet)
date: 2024-05-10 23:55 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given an array arr[] of size n. Find the total count of sub-arrays having their sum equal to 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/zero-sum-subarrays1825/1?page=2)

## SOLUTION

```java
class Solution{

    public static long findSubarray(long[] arr, int n)
    {
        // Create a HashMap to store the cumulative sums and their occurrences.
        Map<Integer, Integer> map = new HashMap<>();

        int count = 0;

        // track of the current sum.
        int currentSum = 0;

        // IMPORTANT:
        // Initialize the HashMap with an entry for 0 sum.
        // This is crucial because if the array starts with one or more zeros,
        // they should be counted as subarrays with sum equal to 0.
        map.put(0, 1);

        // Traverse the array and calculate the cumulative sum.
        for(int i = 0; i < n; i++){

            currentSum += arr[i];

            // If the cumulative sum exists in the HashMap, it means there's a subarray with sum equal to the difference between currentSum and one of its previous occurrences.
            // So the sum of elements between the previous occurance when we had this sum till current element must be 0
            // The same sum could have occurred multiple times, so we need to get the frequency, which is the number of times we got sum equal to currentSum previously
            if(map.containsKey(currentSum)){
                int times = map.get(currentSum);
                count += times;
            }

            // Update the HashMap with the current cumulative sum and its occurrence.
            map.put(currentSum, map.getOrDefault(currentSum, 0) + 1);
        }

        return count;
    }
}
```
