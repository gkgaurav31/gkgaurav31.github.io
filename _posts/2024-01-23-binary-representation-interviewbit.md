---
layout: post
title: Binary Representation (InterviewBit)
date: 2024-01-23 21:33 +0530
author: "Gaurav Kumar"
tags: "java interviewbit mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a number N >= 0, find its representation in binary.

Example:
if N = 6,
binary form = 110

[InterviewBit](https://www.interviewbit.com/problems/binary-representation/)

## SOLUTION

```java
public class Solution {

    public String findDigitsInBinary(int A) {

        if(A == 0) return "0";

        StringBuffer sb = new StringBuffer();

        while(A != 0){

            sb.insert(0, A%2);
            A = A/2;

        }

        return sb.toString();

    }

}
```
