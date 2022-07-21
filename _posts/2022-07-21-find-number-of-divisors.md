---
layout: post
title: Find Number of Divisors
date: 2022-07-21 21:50 +0530
author: "Gaurav Kumar"
tags: "java prime_numbers important"
categories: "prime_numbers"
---

## Problem Description

Given a number N, find the number of divisors of that number

### Solution

To solve this problem optimally, we will make use of [Smallest Prime Factor]({% post_url 2022-07-21-smallest-prime-factor %}) algorithm covered previously.  

- First we get the Smallest Prime Factor map that we created before. This contains the smallest factor for each number.
- 72 = (2^3 * 3^2)
- The total factors are: (3+1)*(2+1)=12
- The prime factorization of 72 is: 72 -> divide by 2 -> 36 -> divide by 2 -> 18 -> divide by 2 -> 9 -> divide by 3 -> 3 -> divide by 3 -> 1
- If we observe carefully, at every step, we are taking the Smallest Prime Factor of that number and dividing by it. We can use our SPF hashmap to get the the value. Once we've got this, we will keep checking if N is divisible by current spf. If it is, increase counter by 1 and divide N by current spf. If N is not divisible by spf, that means, we will need to calculate the spf again. Along with that we add this to our total -> total = total * (count+1)

```java
//Given a number N, find the number of divisors of that number
private static int numberOfDivisors(int n) {

    Map<Integer, Integer> spf = smallestPrimerFactor(n);

    int total=1;

    while(n > 1){
        int count = 0;
        int p = spf.get(n);
        while(n%p == 0){
            count++;
            n = n/p;
        }
        total = total * (p+1);
    }

    return total;
}

//Given N, find the smallest prime factor for all numbers from [2-N]
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
