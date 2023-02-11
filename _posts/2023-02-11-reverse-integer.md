---
layout: post
title: Reverse Integer
date: 2023-02-11 20:05 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 bit_manipulation important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

[leetcode](https://leetcode.com/problems/reverse-integer/description/)

### Solution

[NeetCode YouTube](https://www.youtube.com/watch?v=HAgLH58IgJQ)

```java
class Solution {

    public int reverse(int n) {

        int ans = 0;
        
        //while N is not 0, get the last digit and add it to ans while checking that the current value does not overflow
        while(n != 0){

            //last digit
            int d = n%10;
            //chop off the last digit from N
            n = n/10;

            //if the current ans is more than MAX/10 or current ans is equal to MAX && last digit is more than the digit in MAX_VALUE, return 0
            //similarly for min value
            if(ans > Integer.MAX_VALUE/10 || (ans == Integer.MAX_VALUE/10 && d > Integer.MAX_VALUE%10)) return 0;
            if(ans < Integer.MIN_VALUE/10 || (ans == Integer.MIN_VALUE/10 && d < Integer.MIN_VALUE%10)) return 0;

            ans = (ans * 10) + d;

        }

        return ans;

    }

}
```
