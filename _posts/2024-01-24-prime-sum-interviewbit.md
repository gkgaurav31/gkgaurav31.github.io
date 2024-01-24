---
layout: post
title: Prime Sum (InterviewBit)
date: 2024-01-24 20:43 +0530
author: "Gaurav Kumar"
tags: "java prime_numbers important"
categories: "prime_numbers"
---

## PROBLEM DESCRIPTION

Given an even number ( greater than 2 ), return two prime numbers whose sum will be equal to the given number.
If there is more than one solution possible, return the lexicographically smaller solution i.e.

If [a, b] is one solution with a <= b,  
and [c,d] is another solution with c <= d, then

[a, b] < [c, d]  
If a < c OR ( a == c AND b < d ).

NOTE: A solution will always exist. read Goldbach's conjecture

### SOLUTION

- We get a list of prime number from `[2, A]`
  - For this, we start by creating a boolean array `prime[]` of size A+1 to check if ith number is prime or not. We initialize all of them with true.
  - Next, we mark prime[0] and prime[1] to false
  - We start the iteration from prime[2]. The value is true, which means it is prime.
  - Any multiple of current value cannot be prime. So, we go ahead and mark all the multiples of 2 to false.
  - We continue to the next number which is 3. This will again be true which means it is prime. We mark all the multiples of 3 as false.
  - 4 would have been marked to false when we were checking 2.
  - 5 will remain prime and we will mark all multiples of 5 as false.
  - We can continue to do this until we have marked all the numbers as prime or not.
- Interate over `prime[]` from index 2 which is the smaller prime number. Consider it as the candidate first number `x`. So, the second number will be `A-x`. We need `A-x` to also be a prime number. We can check this easily now using `prime[A-x]`. Once this is satisfied, we can return this pair as the answer.

```java
public class Solution {

    public int[] primesum(int A) {

        boolean[] prime = getPrimeNumbers(A);

        for(int i=2; i<prime.length; i++){

            int firstNumber = i;
            int secondNumber = A-i;

            if(prime[firstNumber] == true && prime[secondNumber] == true)
                return new int[]{firstNumber, secondNumber};

        }

        return new int[]{};

    }

    public boolean[] getPrimeNumbers(int A){

        boolean[] prime = new boolean[A+1];
        Arrays.fill(prime, true);

        prime[0] = false;
        prime[1] = false;

        for(int i=2; i<=A; i++){

            if(prime[i] == true){

                for(int j= i*2; j<=A; j+=i){
                    prime[j] = false;
                }

            }

        }

        return prime;

    }

}
```
