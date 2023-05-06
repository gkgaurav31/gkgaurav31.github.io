---
layout: post
title: Count Total Set Bits Till N
date: 2023-05-06 20:52 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation leetcode important"
categories: "bit_manipulation"
---

### PROBLEM DESCRIPTION

You are given a number N. Find the total number of setbits in the numbers from 1 to N.

[https://www.geeksforgeeks.org/count-total-set-bits-in-all-numbers-from-1-to-n/](https://www.geeksforgeeks.org/count-total-set-bits-in-all-numbers-from-1-to-n/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/count_set_bits_till_n.png)

```java
class Solution{
    
    static int countBits(int A){
        
        //base case
        if(A == 0) return 0;

        //highest power of 2, which is less than or equal to A
        long x = logBase2(A);

        //number of 1s present in each bit position = 2^x / 2
        long numerOfOnesInEachPosition = (1<<x)/2;

        //how many positions have any set bits? 
        long totalNumberOfPositions = x;

        //all the remaining numbers from 2^x to A will have set bit in its MSB
        long msbContributionForRemainingNumbers = (A - (1<<x) + 1);

        //current total = (total number of set bits upto 2^x - 1) + (set bits in the MSB in the remaining numbers)
        long currentTotal = (numerOfOnesInEachPosition * totalNumberOfPositions) + msbContributionForRemainingNumbers;

        //if we carefully observe, (A / 2^x) is now a sub-problem. So, calculate that using recursion and add that to the currentTotal
        long contributionByRemaining = countBits( A - (1<<x) );

        return (int)(currentTotal + contributionByRemaining); 
    }
    
    public static long logBase2(long n){

        long c = 0;

        while(n > 1){
            c++;
            n = n/2;
        }

        return c;

    }
}
```
