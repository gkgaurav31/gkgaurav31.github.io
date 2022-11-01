---
layout: post
title: Fibonacci Number
date: 2022-11-02 01:03 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given a positive integer A, write a program to find the Ath Fibonacci number.

In a Fibonacci series, each term is the sum of the previous two terms and the first two terms of the series are 0 and 1. i.e. f(0) = 0 and f(1) = 1. Hence, f(2) = 1, f(3) = 2, f(4) = 3 and so on.

NOTE: 0th term is 0. 1th term is 1 and so on.

### Solution

```java
import java.lang.*;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        int[] dp = new int[n+1];
        for(int i=0; i<dp.length; i++) dp[i] = -1;

        System.out.println(fibo(n, dp));
        
    }

    public static int fibo(int n, int dp[]){
        if(n <= 1) return n;
        if(dp[n] == -1){
            dp[n] = fibo(n-1, dp) + fibo(n-2,dp);
        }
        return dp[n];
    }
}
```
