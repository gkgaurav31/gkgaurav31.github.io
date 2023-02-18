---
layout: post
title: Happy Number
date: 2023-02-18 17:25 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 mathematics"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
- Those numbers for which this process ends in 1 are happy.

Return true if n is a happy number, and false if not.

[leetcode](https://leetcode.com/problems/happy-number/description/)

### Solution

```java
class Solution {

    public boolean isHappy(int n) {
        
        Set<Integer> set = new HashSet<>();

        //while the number has not already been seen before, keep adding it to the set
        while(!set.contains(n)){
            set.add(n);
            n = sumOfSquaresOfDigits(n);
        }

        return n==1;

    }

    //calculate sum of squares of each digits in N
    public int sumOfSquaresOfDigits(int n){
        int total = 0;

        while(n!=0){
            total = total + (int)Math.pow(n%10, 2);
            n = n/10;
        }

        return total;
    }
    
}
```
