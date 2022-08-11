---
layout: post
title: Special Integer - Maximum Sum with Max Length k
date: 2022-08-12 02:16 +0530
author: "Gaurav Kumar"
tags: "java search important"
categories: "search"
---

## Problem Description

Given an array of +ve integers, find maximum k such that:
{max subarray sum of length k <= B}, where B is a given input +ve integer.

### Solution

We have given a number B and an array. There can be many subarrays with sum <= B. We need to return the length of subarray which is the maximum of all. We know that we can get the length of subarray which has the maximum sum using sliding window technique. If we do not have any subarray with sum <= B, obviously, it does not make sense to check for higher length subarrays. If there a subarray of length X which has a sum <= B, it can be our potential answer. So, using binary search, we save that answer, and look for better/higher answer such that the length of subarray is maximum.

```java
package com.gauk;

public class SpecialInteger {

    public static void main(String[] args) {
        System.out.println(new SpecialInteger().findSpecialInteger(new int[]{3,2,5,4,6,3,7,2}, 20));
    }

    //Given an array of +ve integers, find maximum k such that:
    //{max subarray sum of length k <= B}, where B is a given input +ve integer.
    public int findSpecialInteger(int arr[], int B){

        int l = 1; //the lowest possible length is 1
        int h = arr.length; //the largest possible length is the length of the array
        int ans = 0; //the least possible answer is 0

        while(l<=h){
            int k = (l+h)/2;
            if(maxSumSubArrayOfLengthK(arr, k) <= B){
                //if there is some subarray with length k whose sum is <=B
                ans = k;
                l = k + 1; //save and look for better answer
            }else{
                //if length k is not possible, and k+1 will not be possible as well, since all are +ve numbers
                h = k - 1;
            }
        }

        return ans;

    }

    public int maxSumSubArrayOfLengthK(int[] arr, int k){

        //if k elements are not present in the array, return 0
        if(arr.length < k) return 0;

        int maxSum = Integer.MIN_VALUE;
        int currentSum = 0;

        //initialize with sum of first k elements
        for(int i=0; i<k; i++){
            currentSum+=arr[i];
        }

        //using sliding window
        int start = 0;
        int end = k;

        while(end < arr.length-1){
            start++;
            end++;
            currentSum+=arr[end];
            currentSum-=arr[start-1];

            if(currentSum > maxSum) maxSum = currentSum;
        }

        return maxSum;
    }

}
```
