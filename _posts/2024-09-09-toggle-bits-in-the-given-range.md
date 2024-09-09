---
layout: post
title: Toggle bits in the given range (geeksforgeeks - SDE Sheet)
date: 2024-09-09 18:47 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a non-negative number n and two values l and r. The problem is to toggle the bits in the range l to r in the binary representation of n, i.e., to toggle bits from the lth least significant bit to the rth least significant bit (the rightmost bit as counted as the first bit).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/toggle-bits-given-range0952/1?page=5)

## SOLUTION

```java
class Solution {

    static int toggleBits(int n , int l , int r) {

        for(int i=l; i<=r; i++){
            n = (n ^ (1 << i-1));
        }

        return n;

    }

};
```
