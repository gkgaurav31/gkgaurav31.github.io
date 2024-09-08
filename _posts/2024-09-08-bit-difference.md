---
layout: post
title: Bit Difference (geeksforgeeks - SDE Sheet)
date: 2024-09-08 19:33 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given two numbers A and B. The task is to count the number of bits needed to be flipped to convert A to B.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/bit-difference-1587115620/1?page=5)

## SOLUTION

```java
class Solution {

    public static int countBitsFlip(int a, int b) {

        // XOR of A and B will give a number where the bits set to '1' represent
        // positions where A and B have different bits.
        int n = a ^ b;

        int count = 0;

        // Loop through each bit of n
        while(n != 0) {

            // Check if the least significant bit (rightmost bit) is set to '1'
            // If so, it means the corresponding bits of A and B differ at that position
            if( (n & 1) != 0 ) {
                count++;
            }

            // Right shift n by 1 to check the next bit in the next iteration
            n = n >> 1;
        }

        return count;
    }
}
```
