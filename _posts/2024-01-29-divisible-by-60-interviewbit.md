---
layout: post
title: Divisible by 60 (InterviewBit)
date: 2024-01-29 22:20 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a large number represent in the form of an integer array A, where each element is a digit.
You have to find whether there exists any permutation of the array A such that the number becomes divisible by 60.
Return 1 if it exists, 0 otherwise.

## SOLUTION

To determine if a number is divisible by 60, which factors into `2^2 * 3 * 5`, certain conditions must be met.

- Divisibility by 5: The last digit of the number must be either 0 or 5.
- Divisibility by 4: The last two digits of the number must be divisible by 4, meaning the second-to-last digit must be even and the last digit should be 0.
- Divisibility by 3: The sum of the digits must be divisible by 3.

In summary, for a number to be divisible by both 4 and 5, the last digits should be 0, and the second-to-last digit should be even. Additionally, for divisibility by 3, the sum of the digits must be divisible by 3. If these conditions are met in an array, there exists a permutation that makes the number divisible by 60.

```java
public class Solution {

    public int divisibleBy60(int[] A) {

        // Edge case: If the array contains only a single element, which is 0
        if(A.length == 1 && A[0] == 0)
            return 1;

        if(sumDivisibleBy3(A) && containsZero(A) && secondEvenFound(A))
            return 1;
        else
            return 0;
    }

    // Method to check if the sum of digits in the array is divisible by 3
    public boolean sumDivisibleBy3(int[] A){
        int s = 0;
        for(int i=0; i<A.length; i++) s += A[i];
        return s % 3 == 0;
    }

    // Method to check if the array contains at least one 0
    public boolean containsZero(int[] A){
        for(int i=0; i<A.length; i++)
            if(A[i] == 0)
                return true;

        return false;
    }

    // Method to check if there are at least two even digits in the array
    public boolean secondEvenFound(int[] A){

        int c = 0;
        for(int i=0; i<A.length; i++){
            if(A[i] % 2 == 0) c++;
            if(c > 1) return true;  // Return true if more than one even digit is found
        }

        return false;
    }
}
```
