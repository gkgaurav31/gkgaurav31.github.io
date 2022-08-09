---
layout: post
title: Ath Magical Number
date: 2022-08-10 00:59 +0530
author: "Gaurav Kumar"
tags: "java search important"
categories: "search"
---

## Problem Description

Given A, B and C, find Ath magical number.  
Note: A number is said to be magical, if it's divisible by B or C.

### Solution

```java
package com.gauk;

public class MagicNumberModded {

    public static void main(String[] args) {
        System.out.println(AthMagicNumber(5, 6, 3)); //10
    }

    //Given A, B and C, find Ath magical number.
    //Note: A number is said to be magical, if it's divisible by B or C.
    static long AthMagicNumber(long B, long C, long A){

        //target: if we take a minimum of B and C and keep multiplying A times, we are sure that we will get at least A magical numbers. Our answer will be less than or equal to this.
        //It can be lesser also, since there will be certain numbers which are multiples of the other number.
        long l = 1; //We can also start with the min(B,C)
        long h = Math.min(B, C) * A; //Max possible answer, since this has at least A multiples of min number.
        long ans = h;

        while(l<=h){

            long m = (l+h)/2;

            long magicalNumbersTillHere = getMagicalNumbersTillHere(B, C, A, m);

            if(magicalNumbersTillHere == A){
                //if number of magical numbers till mid is equal to A, then this is a potential answer.
                //we should continue to check on left because the same number of magical numbers can be present on left side
                ans = m;
                h = m-1;
            }else if(magicalNumbersTillHere < A){
                //go to right
                l = m+1;
            }else{
                //go to left
                h = m-1;
            }

        }

        return ans;

    }

    static long getMagicalNumbersTillHere(long B, long C, long A, long m){

        long x = m/B; //number of multiples of B till m
        long y = m/C; //number of multiples of C till m

        long lcm = lcm(B,C); //lcm of B and C will be common till m
        long z = m/lcm; //number of multiples of lcm(B,C)

        long totalMagicalNumbers = x + y - z;

        return totalMagicalNumbers;
    }

    static long lcm(long a, long b)
    {
        return (a / gcd(a, b)) * b;
    }

    static long gcd(long a, long b)
    {
        if (a == 0)
            return b;
        return gcd(b % a, a);
    }

}
```
