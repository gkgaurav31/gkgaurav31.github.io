---
layout: post
title: Sum of Bitwise-OR of all Sub-Arrays
date: 2023-04-30 14:15 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation geeksforgeeks important"
categories: "bit_manipulation"
---

### PROBLEM DESCRIPTION

You are given an array of integers A of size N.
The value of a subarray is defined as BITWISE OR of all elements in it.
Return the sum of value of all subarrays of A % 109 + 7.

[geeksforgeeks](https://www.geeksforgeeks.org/sum-of-bitwise-or-of-all-subarrays-of-a-given-array-set-2/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/subarray_or_sum.png)

```java
public class Solution {

    long M = 1000000007;

    public int solve(int[] A) {

        int n = A.length;

        //total number of sub arrays possible
        long totalSubarrays = sumOfN(n); // n*n+1/2

        //init: total sum (answer)
        long total = 0;

        //for each bit position
        for(int i=0; i<=31; i++){
            
            //get the number of subarrays which have ith bit as unset
            long subArrayUnsetBit = calcSubArrayUnsetBitAtPos(i, A, n);

            //number of sub arrays which have ith bit set
            long subArraySetBit = totalSubarrays - subArrayUnsetBit;
            
            //if ith bit is set, its value would be: 2^i == (1<<i)
            long powerValue = (1<<i);

            //contribution to total sum by all subarrays which has set bit at ith position
            long contribution = (subArraySetBit * powerValue);

            total = (total + contribution);
    
        }

        return (int)(total%M);

    }

    //Calculate the number of sub arrays with unset ith bit position
    public long calcSubArrayUnsetBitAtPos(int position, int[] A, int n){

        //init:
        long total = 0;
        long current = 0;

        //for each element, check if bit at "position" is unset
        //if it's unset, add to the total
        //since we are looking for number of subarrays, if there are continuous unset bit position, the number of subarrays will depend on it as well. See the explanation in the image
        for(int i=0; i<n; i++){

            if(!checkSetAtPosition(position, A[i])){
                current++;
            }else {
                total = total + sumOfN(current);
                current = 0; //reset
            }

        }

        total = total + sumOfN(current);

        return total;

    }

    //check if the ith bit is set in N
    public boolean checkSetAtPosition(int i, int n){
        if( (n&(1<<i)) != 0){
            return true;
        }else
            return false;
    }

    //sum of first N natural numbers
    public long sumOfN(long n){
        return (n*(n+1))/2;
    }

}
```
