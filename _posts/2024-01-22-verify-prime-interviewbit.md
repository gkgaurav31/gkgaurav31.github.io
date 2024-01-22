---
layout: post
title: Verify Prime (InterviewBit)
date: 2024-01-22 22:13 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a number N, verify if N is prime or not.
Return 1 if N is prime, else return 0.

[InterviewBit](https://www.interviewbit.com/problems/verify-prime/)

## SOLUTION

If a is a factor of N, then N/a will also be its factor. So, we do not need to check for all numbers from 1 to N to check if it divides N. We can only go till sqrt(N). One thing to notice is that if N is a perfect square, there will be one a for which N/a is also same. For example: N=25. For a=5, b will also be same.

```java
public class Solution {

    public int isPrime(int A) {

        int count = 0;

        int sqrtA = (int)Math.ceil(Math.sqrt(A));

        for(int i=1; i<=sqrtA; i++){
            if(A%i == 0){

                if(A/i == i) count++;
                else count += 2;

            }

            if(count >= 3) return 0;

        }

        return count==2?1:0;

    }
}
```
