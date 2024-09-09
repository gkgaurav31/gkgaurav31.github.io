---
layout: post
title: K-th Bit is Set or Not (geeksforgeeks - SDE Sheet)
date: 2024-09-09 19:05 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a number n and a bit number k, check if kth index bit of n is set or not. A bit is called set if it is 1. Position of set bit '1' should be indexed starting with 0 from LSB side in binary representation of the number.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/check-whether-k-th-bit-is-set-or-not-1587115620/1?page=5)

## SOLUTION

```java
class CheckBit {
    static boolean checkKthBit(int n, int k) {
        return ( (n&(1<<k)) != 0);
    }
}
```
