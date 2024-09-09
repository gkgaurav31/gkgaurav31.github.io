---
layout: post
title: Longest Consecutive 1's (geeksforgeeks - SDE Sheet)
date: 2024-09-09 19:56 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a number N. Find the length of the longest consecutive 1s in its binary representation.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/longest-consecutive-1s-1587115620/1?page=6)

## SOLUTION

### APPROACH 1

```java
class Solution{

    public static int maxConsecutiveOnes(int n) {

        int max = 0;
        int current = 0;

        while(n != 0){

            if((n&1) != 0){
                current++;
            }else{
                current = 0;
            }

            max = Math.max(max, current);

            n = n >> 1;

        }

        return max;

    }
}
```

### APPROACH 2

By taking the bitwise AND (&) of N and N left-shifted by 1, we effectively "erase" the trailing 1 from every sequence of consecutive 1s. This reduces the number of consecutive 1s in each iteration, helping us count their length.

```java
class Solution{

    public static int maxConsecutiveOnes(int n) {

        int count = 0;

        while(n != 0){

            n = (n & (n << 1)); // right shift would also work with the same logic
            count++;

        }

        return count;

    }
}
```
