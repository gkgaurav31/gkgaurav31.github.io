---
layout: post
title: Find Prime Numbers
date: 2022-07-21 20:32 +0530
author: "Gaurav Kumar"
tags: "java prime_numbers important"
categories: "prime_numbers"
---

## Problem Description

Given a number N, return a list of all prime numbers till N.

### Solution

This can be optimally solved using [Sieve of Eratosthenes](https://www.geeksforgeeks.org/sieve-of-eratosthenes/) algorithm.

- First we creat an boolean array of `N+1` size and mark of them as prime (true)
- We know that 0,1 are not primes, so initialize them first
- Now we start from 2. Since the corresponding value is true, it's prime
- We know that all multiples of this number will not be prime. So, we start from it's 2nd multiple which is i\*2 and keep setting them all as false. In the first iteration 4, 6, 8, 10, 12, 14, 16, 18, 20 ... will all be marked as false
- The value for 3, 5 will be true. This means that there was no value before 5 which is its factor. That means it's a prime number. We don't really need to call isPrime separately. If it's not marked as true while iterating through the numbers, it will be a prime number
- Now just update all the multiples of 5 as false: 10, 15, 20, 25 30 etc.
- We can further optimize this algorithm with a small tweak. If we observe carefully, we update the same value again and again if that number is a factor of multiple prime numbers
- If we look at the pattern, the first number which will be marked as false will be equal to `i*i` (i.e `currentElement*currentElement`)
- So we can simply start our inner loop `j=i*i` instead of `j=i*2`

```java
private static List<Integer> getAllPrimeNumbers(int n) {

    boolean prime[] = new boolean[n+1];

    for(int i=2; i<prime.length; i++){
        prime[i] = true;
    }

    for(int i=2; i*i<=n; i++){
        if(prime[i] == true){
            //i is prime, mark all multiple of i as false
            //the first number that we start marking will be i*i
            for(int j=i*i; j<=n; j=j+i){
                prime[j] = false;
            }
        }
    }

    List<Integer> listOfPrimes = new ArrayList<>();
    for(int i=0; i<prime.length; i++){
        if(prime[i] == true) listOfPrimes.add(i);
    }

    return listOfPrimes;

}
```

:exclamation: The time complexity is: O(N log(logN))
