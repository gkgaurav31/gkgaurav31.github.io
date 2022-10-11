---
layout: post
title: Number of 1 Bits
date: 2022-10-11 13:46 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).  

Note:  

Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer. -3.  

[leetcode](https://leetcode.com/problems/number-of-1-bits/)

### Solution

Read about the difference between Logical Right Shift (&gt;&gt;&gt;) and Arithemetic Right Shift (&gt;&gt;):  
[Right Shifts](https://inst.eecs.berkeley.edu/~cs61bl/r/cur/bits/bit-shifting.html?topic=lab28.topic&step=4&course=)

```java
public class Solution {

    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int c=0;
        while(n!=0){
            if( (n&1) == 1) c++;
            n=(n>>>1);
        }
        return c;
    }

}
```
