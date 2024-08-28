---
layout: post
title: Longest consecutive subsequence (geeksforgeeks - SDE Sheet)
date: 2024-08-28 23:13 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of non-negative integers. Find the length of the longest sub-sequence such that elements in the subsequence are consecutive integers, the consecutive numbers can be in any order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-consecutive-subsequence2449/1?page=3)

## SOLUTION

```java
    class Solution
    {
    // arr[] : the input array
    // N : size of the array arr[]

    //Function to return length of longest subsequence of consecutive integers.
    static int findLongestConseqSubseq(int arr[], int n)
    {

        int maxLength = 0;

        Set<Integer> set = new HashSet<>();

        for(Integer x: arr){
            set.add(x);
        }

        for(int i=0; i<n; i++){

            // MAIN OPTIMIZATION:
            // x = arr[i]
            // if array contains x-1,
            // the longest sequence cannot start from x
            // we will start counting only if we get the starting position
            if(set.contains(arr[i]-1))
            continue;

            int len = 0;
            int current = arr[i];

            while(set.contains(current)){
                len++;
                current++;
            }

            maxLength = Math.max(maxLength, len);

        }

        return maxLength;

    }
    }
```
