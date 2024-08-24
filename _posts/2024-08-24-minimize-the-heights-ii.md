---
layout: post
title: Minimize the Heights II
date: 2024-08-24 11:26 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array `arr[]` denoting heights of `N` towers and a positive integer `K`.

For each tower, you must perform exactly one of the following operations exactly once.

- Increase the height of the tower by `K`
- Decrease the height of the tower by `K`

Find out the minimum possible difference between the height of the shortest and tallest towers after you have modified each tower.

Note: It is compulsory to increase or decrease the height by `K` for each tower. After the operation, the resultant array should not contain any negative integers.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimize-the-heights3351/1?page=1)

## SOLUTION

[Good Explanation - GeeksForGeeks](https://www.youtube.com/watch?v=30vDmZg5MZ8)

```java
class Solution {

    int getMinDiff(int[] arr, int n, int k) {

        Arrays.sort(arr);

        int res = arr[n-1] - arr[0];

        int smallest = arr[0] + k;
        int largest = arr[n-1] - k;

        int mi = 0;
        int ma = 0;

        for(int i=0; i<n-1; i++){

            mi = Math.min(smallest, arr[i+1] - k);
            ma = Math.max(largest, arr[i] + k);

            if(mi < 0) continue;

            res = Math.min(res, ma - mi);

        }

        return res;

    }

}
```
