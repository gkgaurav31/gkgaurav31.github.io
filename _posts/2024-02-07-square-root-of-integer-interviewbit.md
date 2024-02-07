---
layout: post
title: Square Root of Integer (InterviewBit)
date: 2024-02-07 22:49 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer `A`.

Compute and return the square root of `A`.

If `A` is not a perfect square, return `floor(sqrt(A))`.

DO NOT USE SQRT FUNCTION FROM STANDARD LIBRARY.

NOTE: Do not use sort function from standard library. Users are expected to solve this in `O(log(A))` time.

## SOLUTION

We can solve this using binary search. We can start with the range `[0, Integer.MAX_VALUE]` since the answer will definitely lie within this range. Then, we can pick up a middle value and check if its square is equal to the given number. To avoid overflow, we need to use `long` instead of `int` for the `range` and `mid`.

```java
public class Solution {

    public int sqrt(int A) {

        // Range of possible answers for binary search
        long l = 0;
        long r = Integer.MAX_VALUE;

        // To store the floor value in case we never got a perfect square
        long ans = -1;

        while(l<=r){

            // Get current mid
            long m = (l+r)/2;

            // Current mid square
            long mSquare = m*m;

            // If current mid value's square is equal to A, we have a perfect square
            if(mSquare == A){
                return (int) m;

            // If it is more, look towards left
            }else if(mSquare > A){
                r = m-1;

            // Otherwise, look towards right
            // Also store this value as it can potentially be the floor value that we need to return
            }else{
                ans = m;
                l = m+1;
            }

        }

        return (int) ans;

    }

}
```
