---
layout: post
title: Help From Sam
date: 2023-04-25 22:50 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation"
categories: "bit_manipulation"
---

### PROBLEM DESCRIPTION

Alex and Sam are good friends. Alex is doing a lot of programming these days. He has set a target score of A for himself.
Initially, Alex's score was zero. Alex can double his score by doing a question, or Alex can seek help from Sam for doing questions that will contribute 1 to Alex's score. Alex wants his score to be precisely A. Also, he does not want to take much help from Sam.

Find and return the minimum number of times Alex needs to take help from Sam to achieve a score of A.

### SOLUTION

If A is an even number we can divide it by 2. Otherwise if A is an odd number we can subtract 1 from it and increment the count. If we observe carefully, this will turn out to the number of set bits in A.

```java
public class Solution {

    public int solve(int A) {

        int c = 0;

        while(A > 0){
            if((A&1) == 1) c++;
            A = A>>1;
        }

        return c;

    }

}

```
