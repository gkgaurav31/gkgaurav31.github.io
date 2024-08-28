---
layout: post
title: Count distinct elements in every window (geeksforgeeks - SDE Sheet)
date: 2024-08-28 22:21 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of integers and a number K. Find the count of distinct elements in every window of size K in the array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/count-distinct-elements-in-every-window/1?page=3)

## SOLUTION

```java
class Solution {

    ArrayList<Integer> countDistinct(int A[], int n, int k) {

        ArrayList<Integer> result = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();

        // Initialize the map with the first window
        for (int i = 0; i < k; i++) {
            map.put(A[i], map.getOrDefault(A[i], 0) + 1);
        }

        // Add the count of distinct elements in the first window
        result.add(map.size());

        // Slide the window over the array
        for (int i = k; i < n; i++) {

            // Remove the element going out of the window
            int outgoing = A[i - k];
            if (map.get(outgoing) == 1) {
                map.remove(outgoing);
            } else {
                map.put(outgoing, map.get(outgoing) - 1);
            }

            // Add the new element coming into the window
            int incoming = A[i];
            map.put(incoming, map.getOrDefault(incoming, 0) + 1);

            // Add the count of distinct elements in the current window
            result.add(map.size());

        }

        return result;
    }
}

```
