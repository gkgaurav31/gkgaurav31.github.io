---
layout: post
title: Power Function
date: 2022-06-15 23:51 +0530
author: "Gaurav Kumar"
tags: "java miscelleneous important"
categories: "miscelleneous"

---

## Problem Description

Given integers A and N, find A power N. (Recursive Approach)

[leetcode](https://leetcode.com/problems/powx-n/description/)

## Solution

A simple way to solve recursively is by doing something like:

```java
public static int calc(int a, int n) {

    if(n == 0) return 1;
    return a * calc(a, n-1);

}
```

This works but it takes O(n) time. We can optimize it futher using the following approach, which gives us the power in __O(logN)__ time.

![snapshot]({{ site.baseurl }}/assets/img/power.jpg)

```java
package recursion;

public class Power {

public static void main(String[] args) {
    System.out.println(pow(3, 3));
}

public static int pow(int a, int n) {
    
    if(n == 0) return 1;
    
    int halfExponent = pow(a, n/2);
    int halfAnswer = halfExponent*halfExponent;
    
    if(n%2 == 0) {
        return halfAnswer;
    }else {
        return halfAnswer*a;
    }
    
}

}
```

### Handling Negative Powers

```java
class Solution {

    public double myPow(double a, long n) {

        if(n == 0) return 1;

        long N = n;
        if (N < 0) {
            a = 1 / a;
            N = -N;
        }

        double halfExponent = myPow(a, N/2);
        double halfAnswer = halfExponent*halfExponent;
        
        if(N%2 == 0) {
            return halfAnswer;
        }else {
            return halfAnswer*a;
        }

    }
    
}
```
