---
layout: post
title: Array Subset of another array (geeksforgeeks - SDE Sheet)
date: 2024-04-04 23:14 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two arrays: `a1[0..n-1]` of size `n` and `a2[0..m-1]` of size `m`, where both arrays may contain duplicate elements. The task is to determine whether array `a2` is a subset of array `a1`. It's important to note that both arrays can be sorted or unsorted. Additionally, each occurrence of a duplicate element within an array is considered as a separate element of the set.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/array-subset-of-another-array2317/1?page=1&sprint=a663236c31453b969852f9ea22507634&sortBy=submissions)

## SOLUTION

```java
class Compute {

    public String isSubset(long a1[], long a2[], long n, long m) {

        // Create a HashMap to store the frequency of elements in array a1
        Map<Long, Integer> map = new HashMap<>();

        // Iterate through array a1 to populate the HashMap with element frequencies
        for (int i = 0; i < n; i++) {
            map.put(a1[i], map.getOrDefault(a1[i], 0) + 1); // Increment frequency count
        }

        // Iterate through array a2 to check if each element exists in a1 and its frequency is sufficient
        for (int i = 0; i < m; i++) {

            // If the current element in a2 doesn't exist in a1, it's not a subset
            if (!map.containsKey(a2[i]))
                return "No";

            // Decrement the frequency count of the current element in a2 in the HashMap
            map.put(a2[i], map.get(a2[i]) - 1);

            // If the frequency count becomes negative, it means a2 has more occurrences of an element than a1
            if (map.get(a2[i]) == -1)
                return "No";
        }

        // If all elements in a2 are found in a1 with sufficient frequency, then a2 is a subset of a1
        return "Yes";
    }
}
```
