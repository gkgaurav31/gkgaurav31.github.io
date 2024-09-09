---
layout: post
title: Rightmost different bit (geeksforgeeks - SDE Sheet)
date: 2024-09-09 19:30 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two numbers M and N. The task is to find the position of the rightmost different bit in the binary representation of numbers. If both M and N are the same then return -1 in this case.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rightmost-different-bit-1587115621/1?page=6)

## SOLUTION

```java
class Solution
{
    //Function to find the first position with different bits.
    public static int posOfRightMostDiffBit(int m, int n)
    {

        // right most bit which is diff will be 1 in the XOR (other bits on left of it can also be 1)
        int x = (n ^ m);

        if(x == 0)
            return -1;

        // x&-x will give us only the number which has the right most set bit
        // we can get that position using log base 2
        return 1 + (int)(Math.log(x&-x)/Math.log(2));

    }
}
```
