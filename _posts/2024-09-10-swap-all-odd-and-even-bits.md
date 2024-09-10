---
layout: post
title: Swap all odd and even bits (geeksforgeeks - SDE Sheet)
date: 2024-09-10 21:38 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an unsigned integer N. The task is to swap all odd bits with even bits. For example, if the given number is 23 (00010111), it should be converted to 43(00101011). Here, every even position bit is swapped with an adjacent bit on the right side(even position bits are highlighted in the binary representation of 23), and every odd position bit is swapped with an adjacent on the left side.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/swap-all-odd-and-even-bits-1587115621/1?page=6)

## SOLUTION

```java
class Solution
{

    public static int swapBits(int n)
    {

        // init:
        // evenBit: num which has a set bit for all even positions
        // oddBit: num which has a set bit for all odd positions
        int evenBit = 0b10101010101010101010101010101010; // == Integer.parseInt("10101010101010101010101010101010", 2);
        int oddBit  = 0b01010101010101010101010101010101;

        // retain only the even positions
        evenBit = (evenBit & n);

        // retain only the odd positions
        oddBit = (oddBit & n);

        // after swapping, the even bit will be right shifted by 1 position
        int evenBitShifted = evenBit >> 1;

        // after swapping, the odd bit will be left shifted by 1 position
        int oddBitShifted = oddBit << 1;

        // the result will be a combination of the left and right shifted values
        return evenBitShifted | oddBitShifted;

    }

}
```
