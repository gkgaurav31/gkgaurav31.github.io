---
layout: post
title: Minimum XOR value pair (geeksforgeeks - SDE Sheet)
date: 2024-10-13 17:42 +0530
author: "Gaurav Kumar"
tags: "java trie geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of integers of size N find minimum xor of any 2 elements.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-xor-value-pair/1?page=9)

## SOLUTION

The idea is to sort the array and then find the xor of all the number iteratively and calculate the minimum at every iteration.

XOR between two numbers is smaller when their binary representations are more similar. Numbers that are numerically close are also more likely to have similar binary forms, especially in the most significant bits.

```java
class Solution{

    static int minxorpair(int N, int arr[]){

        Arrays.sort(arr);

        int min = Integer.MAX_VALUE;

        for(int i=1; i<N; i++){
            min = Math.min(min, arr[i]^arr[i-1]);
        }

        return min;

    }

}
```
