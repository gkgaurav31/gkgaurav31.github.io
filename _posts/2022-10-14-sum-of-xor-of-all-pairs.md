---
layout: post
title: Sum of XOR of all pairs
date: 2022-10-14 00:48 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks important"
categories: "bit_manipulation"
---

## Problem Description

Given an array of N integers, find the sum of xor of all pairs of numbers in the array.
[geeksforgeeks](https://practice.geeksforgeeks.org/problems/sum-of-xor-of-all-pairs0723/1)

### Solution

```java
class Solution{
   
    // Function for finding maximum and value pair
    public long sumXOR (int arr[], int n) {
        
        long sum = 0;
        
        for(int i=0; i<=31; i++){
            
            long c = 0; //Count of number of set bits at position i

            for(int j=0; j<n; j++){
                if(checkSetAtPosition(arr[j], i)) c++; //Increase count if the bit is set at position i
            }
            //The number of pairs which can be made with numbers which have bits set at pos i AND numbers which have bit unset at pos i = c*(n-c)
            //Every pair will contribute 2^i where i is its position. 2^1 === (1<<i)
            //So, we add the contribution on this position to the total sum
            sum = sum + c*(n-c)*(1<<i);
        }
        
        return sum;
        
    }
    
    public boolean checkSetAtPosition(int n, int i){
        n = n>>i;
        return (n&1)==1;
    }
    
    
}
```

