---
layout: post
title: Min XOR value (InterviewBit)
date: 2024-02-19 20:25 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array A of N integers, find the pair of integers in the array which have minimum XOR value. Report the minimum XOR value.

## SOLUTION

By sorting the array, we ensure that adjacent elements are closest to each other in value. This is crucial because XORing two numbers with similar values will result in a smaller XOR value compared to XORing two numbers with larger differences. Sorting the array enables us to efficiently compare adjacent elements.

```java
public class Solution {

    public int findMinXor(int[] A) {

        Arrays.sort(A);

        int n = A.length;
        int ans = Integer.MAX_VALUE;

        for(int i=0; i<n-1; i++){
            ans = Math.min(ans, (A[i]^A[i+1]));
        }

        return ans;

    }

}
```
