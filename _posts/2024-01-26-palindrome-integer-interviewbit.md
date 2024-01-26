---
layout: post
title: Palindrome Integer (InterviewBit)
date: 2024-01-26 23:06 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Determine whether an integer is a palindrome. Do this without extra space.
A palindrome integer is an integer x for which reverse(x) = x where reverse(x) is x with its digit reversed. Negative numbers are not palindromic.

## SOLUTION

One straight forward solution is to conver the int A into a String and compare check if it's a palindrome by taking two pointers from left and right. However, we are not allowed to use extra space. So, we would need to find another way to reverse the given int without conversion.

If we know the last digit `d` and the number of digits `c`, we can place it to the position with highest value by using `d * 10^(c-1)`.

For example: `A = 567`

We have the last digit `7`. Number of digits `c` is `3`.

To place it on the highest position, we can simply do: `7 x 10^2`.

For the next digit `6` add `6 x 10^1` and finally `5 x 10^0`.

```java
public class Solution {

    public int isPalindrome(int A) {
        if(A < 0) return 0;
        return A == reverse(A) ? 1 : 0;
    }

    public int numberOfDigits(int A){

        int count = 0;

        while(A != 0){
            count++;
            A = A/10;
        }

        return count;

    }

    public int reverse(int A){

        int res = 0;
        int pow = numberOfDigits(A)-1;

        while(A != 0){

            int d = A%10;
            A = A/10;

            res = res + d * (int)Math.pow(10, pow);

            pow--;

        }

        return res;

    }
}
```
