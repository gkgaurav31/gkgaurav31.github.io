---
layout: post
title: Array Sum (InterviewBit)
date: 2024-01-18 23:05 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given two numbers represented as integer arrays A and B, where each digit is an element.
You have to return an array which representing the sum of the two given numbers.

The last element denotes the least significant bit, and the first element denotes the most significant bit.

Note : Array A and Array B can be of different size. ( i.e. length of Array A may not be equal to length of Array B ).

[InterviewBit](https://www.interviewbit.com/problems/array-sum/)

## SOLUTION

```java
public class Solution {

    public int[] addArrays(int[] A, int[] B) {

        int n1 = A.length;
        int n2 = B.length;

        int carry = 0;

        // StringBuffer to store the result
        StringBuffer sb = new StringBuffer();

        // Initialize indices for array traversal from right to left
        int i = n1-1;
        int j = n2-1;

        // Variable to store the next digit in the sum (from right to left)
        int nextDigit = 0;

        // Continue the loop until both arrays are completely traversed and there is no carry left
        while(i >= 0 || j >= 0 || carry > 0){

            // Extract digits from each array
            // if i or j becomes less than 0, that means no remaining digits are present for it
            // but the other number or even carry might still have some value
            int d1 = i<0 ? 0 : A[i];
            int d2 = j<0 ? 0 : B[j];

            // Calculate the sum of digits and the carry for the next iteration
            int s = d1 + d2 + carry;

            nextDigit = s % 10;   // this will be the next digit to be added
            carry = s / 10;       // Update carry for the next iteration

            // Insert the digit at the beginning of the result buffer
            sb.insert(0, String.valueOf(nextDigit));

            // Move to the next digits in both arrays
            i--; j--;
        }

        // Convert the result from StringBuffer to an array of integers
        int[] ans = new int[sb.length()];
        for(int k = 0; k < sb.length(); k++){
            ans[k] = Integer.parseInt(sb.charAt(k) + "");
        }

        return ans;
    }
}
```
