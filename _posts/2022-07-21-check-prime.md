---
layout: post
title: Check Prime
date: 2022-07-21 20:29 +0530
author: "Gaurav Kumar"
tags: "java prime_numbers"
categories: "prime_numbers"
---

## Problem Description

Given a number N check if it's a prime number.

### Solution

The main this to remember here is that we need to check only will root of N, because if i is a factor of N, N/i will also be a factor of N. If we get any 1 factor, return false.

```java
static boolean isPrime(int n){

    for(int i=2; i*i<=n; i++){  //We can check till root N only. If i is a factor, N/i will also be a factor.
        if(n%i == 0){
            return false;
        }
    }

    return true;
}
```