---
layout: post
title: Count total set bits (geeksforgeeks - SDE Sheet)
date: 2024-10-16 22:05 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a number n. Find the total count of set bits for all numbers from 1 to n (both inclusive).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/count-total-set-bits-1587115620/1?page=6)

## SOLUTION

### BRUTE-FORCE (TLE)

```java
class Solution{

    //Function to return sum of count of set bits in the integers from 1 to n.
    public static int countSetBits(int n){

        int c = 0;

        for(int i=1; i<=n; i++){
            c+=count(i);
        }

        return c;

    }

    public static int count(int n){

        int c = 0;

        while(n != 0){
            c++;
            n = n&(n-1);
        }

        return c;

    }

}
```

### APPROACH 2 (IDENTIFYING PATTERN)

- The important observation here is that for numbers up to `2^i - 1`, the number of set bits in each position is the same. For example, with `N = 11`, the number of set bits for all valid positions is `4`.
- First, we need to figure out `x` — the largest power of 2 such that `2^x <= N`. The number of set bits for all valid positions up to `2^x - 1` will be `2^x / 2` for each bit position. The number of bit positions that contribute set bits is equal to `x`. Using this, we can calculate the total number of set bits from `[0, 2^x - 1]`.
- How many numbers remain? `N - 2^x + 1`. All of these will have their most significant bit (MSB) set, so we add this count to the total.
- Observation: The remaining part forms a subproblem that can be solved recursively. In this case, the remaining number is `N - 2^x`.

Refer:
[Count total set bits till N]({% post_url 2023-05-06-count-total-set-bits-till-n %})

```java
class Solution{

    //Function to return sum of count of set bits in the integers from 1 to n.
    public static int countSetBits(int n){

        if(n == 0)
            return 0;

        // highest power of 2 <= n
        int x = logBase2(n);

        // number of set bits in each position
        int setBitsCountPerPosition = (1<<x) / 2;

        int numberOfPositions = x;

        int setBitCountOfMSBForRemainingNumbers = n - (1<<x) + 1;

        int currentTotal = setBitsCountPerPosition*numberOfPositions + setBitCountOfMSBForRemainingNumbers;

        int subProblem = n - (1 << x);

        return currentTotal + countSetBits(subProblem);

    }

    public static int logBase2(long n){
        int c = 0;
        while(n > 1){
            c++;
            n = n/2;
        }
        return c;
    }

}
```
