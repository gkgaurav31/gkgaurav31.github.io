---
layout: post
title: First Set Bit (geeksforgeeks - SDE Sheet)
date: 2024-09-09 19:11 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an integer N. The task is to return the position of first set bit found from the right side in the binary representation of the number.
Note: If there is no set bit in the integer N, then return 0 from the function.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-first-set-bit-1587115620/1?page=5)

## SOLUTION

### APPROACH 1

```java
class Solution
{
    public static int getFirstSetBit(int n){

        int idx = 1;

        while(n != 0){

            if( (n&1) != 0 ){
                return idx;
            }

            idx++;
            n = n >> 1;

        }

        return 0;

    }
}
```

### APPROACH 2

```java
class Solution
{

    public static int getFirstSetBit(int n){

        if(n == 0)
            return 0;

        // In two's complement, the negative of a number flips all the bits up to and including the first set bit (from the right),
        // and leaves the remaining bits unchanged.
        // By performing the bitwise AND operation between n and -n, all bits except the lowest set bit are turned to 0.
        int x = (n&-n);

        // by default, math.log(x) uses base e
        // we know, log2x = logx/log2, so we could do:
        int idx = (int)(Math.log(x)/Math.log(2));

        return idx + 1;

    }
}
```
