---
layout: post
title: Divide Integers (InterviewBit)
date: 2024-02-24 20:18 +0530
tags: "java bit_manipulation important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Divide two integers `A` and `B`, without using multiplication, division, and mod operator.

Return the floor of the result of the division.Also, consider if there can be overflow cases. For overflow cases, return `INT_MAX`.

Note: `INT_MAX = 2^31 - 1`

## SOLUTION

One thing we should be aware here is:

`(N << i) == N x (2^i)`

For a given dividend and a divisor, we are essentially looking for the following:

`divisor * (Max power of two) which is less than dividend`

For example:

> init: quotient = 0, temp = 0;  
> Dividend = 43, Divisor = 8

We will iterate from `i=31 to i=0` and try to find the largest power of 2 can be use to multiply with the divisor.

`( 8 x (2^3) is not <= 43)`

`( 8 x (2^2) <= 43)`

This tells us that we can definitely multiple 8, at least 4 times to keep it below the dividend 43. So, let us add that to the quotient. Quotient = 4.

Now, we need to do the same thing for the remaining part of the dividend. How much do we already have? We will use `temp` to store that. So, temp will be `8 x (2^2)` = 32.

Next, the ith position will be i=1. We also need to consider temp, since we have already found some portion of the dividend.

`( 32 + 8 x (2^1) is not <= 43)`

`( 32 + 8 x (2^0) <= 43)`

So now temp becomes `32 + 8 x (2^0) = 40`. We can add the number of time we multiplied the divisor to the previous quotient. So, quotient becomes `4 + (2^0) = 5`.

The loop will end after this point, and we can return the quotient as 5.

```java
public class Solution {

    public int divide(int A, int B) {

        long dividend = A;
        long divisor = B;

        // Determine the sign of the result
        // It will be -ve, only when they have diff signs
        int sign = ((dividend < 0) ^ (divisor < 0) ? -1 : 1);

        // Take absolute values of dividend and divisor
        dividend = Math.abs(dividend);
        divisor = Math.abs(divisor);

        // Initialize quotient and a temporary variable
        long quotient = 0;
        long temp = 0;

        // Iterate from 31 to 0 to find the quotient
        for (long i = 31; i >= 0; i--) {

            // divisor << i === divisor * (2 ^ i)
            // Try to find the largest power of 2 that can be used to multiply with the divisor
            // such that the result is less than or equal to the dividend
            if (temp + (divisor << i) <= dividend) {

                // If true, update temp and set the corresponding bit in quotient
                temp = temp + (divisor << i);
                quotient = quotient | (1L << i);

            }
        }

        // Adjust the sign of the quotient
        if (sign == -1)
            quotient = -quotient;

        // Check for overflow cases
        return quotient > Integer.MAX_VALUE || quotient < Integer.MIN_VALUE ? Integer.MAX_VALUE : (int) quotient;
    }
}
```
