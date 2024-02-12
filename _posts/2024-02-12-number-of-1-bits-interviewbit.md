---
layout: post
title: Number of 1 Bits (InterviewBit)
date: 2024-02-12 19:30 +0530
tags: "java bit_manipulation"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Write a function that takes an integer and returns the number of 1 bits it has.

## SOLUTION

Think of each bit in a binary number as a switch that can be either on (1) or off (0). When we subtract 1 from a number, it's like flipping the rightmost switch from on to off and all the switches to its right also flip. So, if we have a number like `1010`, subtracting 1 from it gives us `1001`. When we perform the AND operation between the original number and the result of `(x - 1)`, it turns off the rightmost switch that was on in the original number. This trick effectively clears the rightmost set bit in the number while leaving the other bits unchanged.

```java
public class Solution {

    public int numSetBits(long a) {

        int count = 0;

        while(a != 0){
            count++;
            a = ( a&(a-1) );
        }

        return count;

    }

}
```
