---
layout: post
title: Implement Power Function (InterviewBit)
date: 2024-02-10 18:01 +0530
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Implement `pow(x, n) % d`.

In other words, given `x`, `n` and `d`,

Find `(x^n % d)`

Note that remainders on division cannot be negative. In other words, make sure the answer you return is non-negative integer.

Return the `ans % 10000003`.

## SOLUTION

[Fast Exponentiation](https://www.geeksforgeeks.org/modular-exponentiation-power-in-modular-arithmetic/)

```java
public class Solution {

    public int pow(int x, int n, int d) {
        return (int) powHelper(x, n, d);
    }

    public long powHelper(int x, int n, int d) {

        if (x == 0) // If base is 0, result will be 0 regardless of power
            return 0;

        if (n == 0) // If power is 0, result will be 1
            return 1;

        // Calculate (x^(n/2) % d) recursively
        long halfPow = pow(x, n / 2, d) % d;

        if (n % 2 == 0) {
            // If power is even, return (halfPow * halfPow) % d
            return (halfPow * halfPow + d) % d; // Handle negative remainder by adding d
        } else {
            // If power is odd, return (halfPow * halfPow * x) % d
            return (halfPow * halfPow * x + d) % d; // Handle negative remainder by adding d
        }
    }
}
```
