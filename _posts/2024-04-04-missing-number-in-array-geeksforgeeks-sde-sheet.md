---
layout: post
title: Missing number in array (geeksforgeeks - SDE Sheet)
date: 2024-04-04 23:19 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of size N-1 such that it only contains distinct integers in the range of 1 to N. Find the missing element.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/missing-number-in-array1416/1?page=1)

## SOLUTION

```java
class Solution {

    int missingNumber(int array[], int n) {

        // Initialize x as the size of the array (n)
        int x = n;

        for(int i=0; i<array.length; i++){

            // We want x to be:
            // 1) XOR of all values from 1,2,3,4...N
            // 2) XOR of all values from a[0],a[1]...a[n-1]
            // We know that x ^ x = 0
            // So, if we XOR 1) and 2) from above, only the missing element will be left behind
            x ^= array[i];
            x ^= i+1;

        }

        // After the loop, x will contain the missing element
        return x;
    }

}
```
