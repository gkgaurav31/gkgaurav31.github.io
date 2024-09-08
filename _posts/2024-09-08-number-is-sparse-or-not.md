---
layout: post
title: Number is sparse or not (geeksforgeeks - SDE Sheet)
date: 2024-09-08 19:41 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a number N. The task is to check whether it is sparse or not. A number is said to be a sparse number if no two or more consecutive bits are set in the binary representation.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/number-is-sparse-or-not-1587115620/1?page=5)

## SOLUTION

We can solve this by iterating through the binary representation of the given number and checking if any two consecutive bits are set. Starting from the least significant bit, we track whether the current bit is 1 using a boolean variable. With each iteration, we right shift the number to examine the next bit. If at any point two consecutive 1s are found, we immediately return false since the number is not sparse.

```java
class Solution
{
    public static boolean isSparse(int n)
    {
        // Initially check if the least significant bit (LSB) is set (1).
        boolean set = ((n & 1) != 0);

        // Right shift the number by 1 to examine the next bit.
        n = n >> 1;

        // Continue checking each bit of the number.
        while(n != 0) {

            // If the current bit is set (1) and the previous bit was also set (i.e., two consecutive bits are set).
            if (((n & 1) != 0) && set == true) {
                // Return false because the number is not sparse (two consecutive set bits found).
                return false;
            }

            // Update the 'set' variable to check the next bit in the next iteration.
            set = ((n & 1) != 0);

            // Right shift the number again to move to the next bit.
            n = n >> 1;
        }

        // If no two consecutive set bits are found, the number is sparse, return true.
        return true;
    }
}
```
