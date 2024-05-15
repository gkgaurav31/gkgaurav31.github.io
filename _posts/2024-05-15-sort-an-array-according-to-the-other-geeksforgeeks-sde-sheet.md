---
layout: post
title: Sort an array according to the other (geeksforgeeks - SDE Sheet)
date: 2024-05-15 20:27 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two integer arrays `A1[ ]` and `A2[ ]` of size `N` and `M` respectively. Sort the first array `A1[ ]` such that all the relative positions of the elements in the first array are the same as the elements in the second array `A2[ ]`.
See example for better understanding.

Note: If elements are repeated in the second array, consider their first occurance only.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/relative-sorting4323/1?page=2)

## SOLUTION

```java
import java.util.*;

class Solution {
    // A1[] : the input array-1
    // N : size of the array A1[]
    // A2[] : the input array-2
    // M : size of the array A2[]

    // Function to sort an array according to the other array.
    public static int[] sortA1ByA2(int A1[], int N, int A2[], int M) {

        // Sort the first array A1
        // There can be elements in A1 which are not present in A2. They need to be added in the natural sorted order
        // So, we can sort the array A1 initially and add left over elements (which are not present in A2) at the end
        Arrays.sort(A1);

        // Create a HashMap to count occurrences of elements in A1
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < N; i++) {
            map.put(A1[i], map.getOrDefault(A1[i], 0) + 1);
        }

        // Create an array to store the result
        int[] res = new int[N];

        // Initialize an index to fill the result array
        int idx = 0;

        // Create a HashSet to keep track of unique elements in A2
        // We will use this to quickly check if the element in A1 is present in A2. This will be done for left over elements later
        Set<Integer> set = new HashSet<>();

        // Iterate through elements of A2
        for(int i = 0; i < M; i++) {

            // Add current element of A2 to the set
            set.add(A2[i]);

            // Skip duplicates in A2
            if(i > 0 && A2[i] == A2[i - 1])
                continue;

            // If the current element of A2 exists in A1
            if(map.containsKey(A2[i])) {

                // Get the number of occurrences of current element of A2 in A1
                int times = map.get(A2[i]);

                // Add the current element of A2 to the result array as many times as it appears in A1
                for(int j = 0; j < times; j++) {
                    res[idx++] = A2[i];
                }

            }

        }

        // Add remaining elements of A1 to the result array which are not in A2
        for(int i = 0; i < N; i++) {
            if(!set.contains(A1[i])) {
                res[idx++] = A1[i];
            }
        }

        return res;
    }
}
```
