---
layout: post
title: Reverse Bits (InterviewBit)
date: 2024-02-13 20:48 +0530
tags: "java bit_manipulation"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Reverse the bits of an 32 bit unsigned integer A.

## SOLUTION

### APPROACH 1

```java
public class Solution {

    public long reverse(long a) {

        long res = 0;

        for(int i=0; i<32; i++){

            if( (a&1) != 0){
                res = res | (1L<<(32-i-1));
                // res = res | (1 << (32 - i - 1)) will NOT work
                // because this will be treated as int and it will lose values beyond 2^32
            }

            a = (a>>1);

        }

        return res;

    }

}
```
