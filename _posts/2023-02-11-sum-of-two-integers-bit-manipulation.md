---
layout: post
title: Sum of Two Integers (Bit Manipulation)
date: 2023-02-11 16:24 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 bit_manipulation important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given two integers a and b, return the sum of the two integers without using the operators + and -.

[leetcode](https://leetcode.com/problems/sum-of-two-integers/description/)

### Solution

[NeetCode YouTube](https://www.youtube.com/watch?v=gVUrDV4tZfY)

![snapshot]({{ site.baseurl }}/assets/img/sum_of_two_integers_bit_manipulation.png)

```java
class Solution {
    
    public int getSum(int a, int b) {

        while(b != 0){

            int temp = a&b;

            a = a ^ b;
            b = temp<<1;

        }

        return a;


    }

}
```
