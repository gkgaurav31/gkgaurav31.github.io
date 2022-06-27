---
layout: post
title: Kadane's Algorithm
date: 2022-06-28 00:38 +0530
author: "Gaurav Kumar"
tags: "java arrays important"
categories: "arrays"
---

## Problem Description

Given an array of numbers, find the maximum sum possible for any subarray.

### Solution

We can use Kadane's Algorithm to solve this.

|<-----positive numbers-----><----negative numbers----><------positive numbers------->

The important thing to understand is whether we should take the negative numbers in our subarray. The answer depends on the later positive integers if they have large enough sum to increase the overall value.

Example:  

10 -2 1 -> Here, we should not include the -ve number  
10 -2 5 -> Here, we will get a larger sum if we include -2 since we are getting 5 later  

Two ways to write our code:  

```java
import java.util.*;

class Program {
  public static int kadanesAlgorithm(int[] array) {

    int maxSum = Integer.MIN_VALUE;

    int maxSumTillHere = 0;
    
    for(int i=0; i<array.length; i++){
      maxSumTillHere = Math.max(maxSumTillHere + array[i], array[i]); //Either the sum will increase if we include the next number OR the next number has a larger value
      maxSum = Math.max(maxSum, maxSumTillHere);
    }

    return maxSum;
    
  }
}
```

Second way:

```java
import java.util.*;

class Program {
  public static int kadanesAlgorithm(int[] array) {

    int maxSum = Integer.MIN_VALUE;
    int currentSum = 0;
    
    for(int i=0; i<array.length; i++){
      currentSum += array[i];
      maxSum = Math.max(currentSum, maxSum);
      if(currentSum < 0) currentSum = 0; //If the current sum is negative, reset it to 0.
    }

    return maxSum;
    
  }
}
```
