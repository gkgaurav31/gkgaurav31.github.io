---
layout: post
title: Trailing Zeroes (InterviewBit)
date: 2024-02-12 19:39 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer A, count and return the number of trailing zeroes.

## SOLUTION

### APPROACH 1

```java
public class Solution {

    public int solve(int A) {

        int c = 0;

        for(int i=0; i<32; i++){

            if( (A&1) == 0 ){
                c++;
                A = (A>>1);
            }else
                break;

        }

        return c;

    }

}
```
