---
layout: post
title: Find the element that appears once in sorted array (geeksforgeeks - SDE Sheet)
date: 2024-08-25 20:15 +0530
author: "Gaurav Kumar"
tags: "java xor geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sorted array arr[] of size N. Find the element that appears only once in the array. All other elements appear exactly twice.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-the-element-that-appears-once-in-sorted-array0624/1?page=2)

## SOLUTION

```java
class Solution
{
    int findOnce(int arr[], int n)
    {
        int x = arr[0];
        for(int i=1; i<n; i++)
            x = x ^ arr[i];
        return x;
    }
}
```
