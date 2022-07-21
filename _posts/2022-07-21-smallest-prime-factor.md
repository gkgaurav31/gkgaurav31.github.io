---
layout: post
title: Smallest Prime Factor
date: 2022-07-21 20:44 +0530
author: "Gaurav Kumar"
tags: "java prime_numbers important"
categories: "prime_numbers"
---

## Problem Description

Given N, find the smallest prime factor for all numbers from [2-N]

### Solution

The solution to this problem is similar to how we found out all prime numbers till N using Sieve of Eratosthenes algorithm. We maintain an array of integers of size N+1. We initialize all the elements as the number itself since that is the max possible value of the number is prime. Then we start our loop from 2. Since the spf for 2 is 2, it means that it's a prime number. All multiples of 2 will have spf as 2, so we update them. Since we are looking for a minimum value, we check if the current value is min from the existing one. (Think about what would happen for 6. Both 2 and 3 will try to update spf for 6). We do this till root N times. Then simply loop through the spf array to get the final result.

```java
private static Map<Integer, Integer> smallestPrimerFactor(int n) {

    int[] spf = new int[n+1];

    //set spf for each number as itself since that is the max value possible
    for(int i=0; i<spf.length; i++){
        spf[i] = i;
    }

    for(int i=2; i*i<=spf.length; i++){
        if(spf[i] == i){
            //i is prime
            //mark spf as i for all multiples of i
            for(int j=i*i; j<spf.length; j=j+i){
                spf[j] = Math.min(spf[j], i);
            }
        }
    }

    Map<Integer, Integer> map = new HashMap<>();
    for(int i=2; i<spf.length; i++){
        map.put(i, spf[i]);
    }
    return map;
}
```
