---
layout: post
title: Party of Couples (geeksforgeeks - SDE Sheet)
date: 2024-09-09 19:51 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given an integer array arr[] of size n, representing n number of people in a party, each person is denoted by an integer. Couples are represented by the same number ie: two people have the same integer value, it means they are a couple. Find out the only single person in the party of couples.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/alone-in-couple5507/1?page=6)

## SOLUTION

```java
class Solution{

    static int findSingle(int n, int arr[]){

        int x = 0;

        for(Integer i: arr)
            x ^= i;

        return x;

    }

}
```
