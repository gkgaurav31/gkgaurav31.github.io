---
layout: post
title: Rotate Bits (geeksforgeeks - SDE Sheet)
date: 2024-10-16 19:08 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an integer N and an integer D, rotate the binary representation of the integer N by D digits to the left as well as right and return the results in their decimal representation after each of the rotation.
Note: Integer N is stored using 16 bits. i.e. 12 will be stored as 0000000000001100.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rotate-bits4524/1?page=4)

## SOLUTION

Left Shift:

If we left shift `N` by `D` places to the left, the left most `D` bits should move towards right. The trick is to get those left most `D` bit by right shifting `N` by `(16-N)` times! Then just taken a bitwise `OR` for both of them. We need to ensure that the final result is still in 16 bit, so we do a bitwise `AND` with `(1<<16) - 1`.

Similar process can be followed for right shift as well.

```java
import java.util.ArrayList;

class Solution {

    // Method to rotate the bits of integer N by D positions to the left and right
    ArrayList<Integer> rotate(int N, int D) {

        // Create a list to store the results of the rotations
        ArrayList<Integer> list = new ArrayList<>();

        // Normalize D to ensure it's within the range of 0-15 for a 16-bit representation
        D = D % 16;

        // Left rotation
        // Shift N left by D positions
        int a = (N << D);
        // Shift N right by (16 - D) positions to capture the overflow bits
        int b = (N >> (16 - D));

        // Combine the two results using bitwise OR and ensure only the last 16 bits are kept
        int res = (a | b) & ((1 << 16) - 1);

        // Add the left-rotated result to the list
        list.add(res);

        // Right rotation
        // Shift N right by D positions
        a = (N >> D);
        // Shift N left by (16 - D) positions to capture the overflow bits
        b = (N << (16 - D));

        // Combine the two results using bitwise OR and ensure only the last 16 bits are kept
        res = (a | b) & ((1 << 16) - 1);

        // Add the right-rotated result to the list
        list.add(res);

        // Return the list containing both rotation results
        return list;
    }
}
```
