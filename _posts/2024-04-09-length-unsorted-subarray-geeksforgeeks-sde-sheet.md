---
layout: post
title: Length Unsorted Subarray
date: 2024-04-09 23:58 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays important"
categories: "geeksforgeeks"
---

## PROBLEM DESCRIPTION

Given an unsorted array Arr of size `N`. Find the subarray `Arr[s...e]` such that sorting this subarray makes the whole array sorted.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/length-unsorted-subarray3022/1)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=GvAtQOMr8CQ)

```java
class Solve {

    int[] printUnsorted(int[] arr, int n) {

        // [SORTED][misplaced_UNSORTED_misplaced][SORTED]

        // Let us first try to find the smallest element which is misplaced
        // Since that element is misplaced, it must be smaller than one of the elements on its left

        int small = 0;
        int max = arr[0];

        for(int i=0; i<n; i++){

            if(arr[i] < max){
                small = i;
            }

            max = Math.max(max, arr[i]);

        }

        // If small is already at its correct position, the array must already be sorted
        if(small == 0)
            return new int[]{0, 0};

        // Let us now try to find the largest element which is misplaced
        // Since that element is misplaced, it must be larger than one of the elements on its right

        int large = n-1;
        int min = arr[n-1];

        for(int i=n-1; i>=0; i--){

            if(arr[i] > min){
                large = i;
            }

            min = Math.min(min, arr[i]);

        }

        // the larger element will be on left side, in the unsorted form
        return new int[]{large, small};

    }

}
```
