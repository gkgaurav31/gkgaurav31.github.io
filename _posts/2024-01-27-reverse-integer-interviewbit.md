---
layout: post
title: Reverse integer (InterviewBit)
date: 2024-01-27 23:06 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given an integer N and the task is to reverse the digits of the given integer. Return 0 if the result overflows and does not fit in a 32 bit signed integer.

## SOLUTION

We can get the last digit using `A%10`. We can add it to a long variable `res` in reverse by using `res = res*10 + (A%10)`. This will give us the reverse value. Since `A` can be negative, we can take a absolute value of it while reversing. To handle overflow, we can calculate the result as `long` and then check if it in between `[Integer.MIN_VALUE, Integer.MAX_VALUE]`.

```java
public class Solution {

    public int reverse(int A) {

        long reverseA = 0;
        int sign = (A < 0 ? -1 : 1);

        A = Math.abs(A);

        while(A != 0){

            reverseA = reverseA*10 + (A%10);
            A = A/10;

        }

        if(reverseA < Integer.MIN_VALUE || reverseA > Integer.MAX_VALUE)
            return 0;

        return sign * (int) reverseA;

    }

}
```
