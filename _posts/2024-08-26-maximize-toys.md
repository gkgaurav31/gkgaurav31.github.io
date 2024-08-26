---
layout: post
title: Maximize Toys (geeksforgeeks - SDE Sheet)
date: 2024-08-26 12:25 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[ ] of length N consisting cost of N toys and an integer K depicting the amount with you. Your task is to find maximum number of toys you can buy with K amount.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximize-toys0331/1?page=2)

## SOLUTION

```java
class Solution{
    static int toyCount(int n, int k, int arr[])
    {

        Arrays.sort(arr);

        int s = 0;
        int count = 0;

        for(int i=0; i<n; i++){

            if(s + arr[i] <= k){
                count++;
                s += arr[i];
            }else
                break;

        }

        return count;

    }
}
```
