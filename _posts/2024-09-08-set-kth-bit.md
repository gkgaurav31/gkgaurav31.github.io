---
layout: post
title: Set kth bit (geeksforgeeks - SDE Sheet)
date: 2024-09-08 19:50 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a number N and a value K. From the right, set the Kth bit in the binary representation of N. The position of Least Significant Bit(or last bit) is 0, the second last bit is 1 and so on.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/set-kth-bit3724/1?page=5)

## SOLUTION

```java
class Solution{
    static int setKthBit(int N,int K){
        // code here
        return (N | (1 << K));
    }
}
```
