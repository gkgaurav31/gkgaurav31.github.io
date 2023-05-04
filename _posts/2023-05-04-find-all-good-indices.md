---
layout: post
title: Find All Good Indices
date: 2023-05-04 23:14 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode"
categories: "arrays"
---

### PROBLEM DESCRIPTION

You are given a 0-indexed integer array nums of size n and a positive integer k.

We call an index i in the range k <= i < n - k good if the following conditions are satisfied:

- The k elements that are just before the index i are in non-increasing order.
- The k elements that are just after the index i are in non-decreasing order.

Return an array of all good indices sorted in increasing order.

[leetcode](https://leetcode.com/problems/find-all-good-indices/description/)

### SOLUTION

Let's say, that the array is:  

> [2 11 1 1 4 1]

Let's create an array such as, i will be 1, if the next element is non-increasing:

```java
if(nums[i+1] <= nums[i]){
    nonIncreasing[i] = 1;
}
```

> [0 1 1 0 1 0] (The last one can be 1/0, it does not matter)

If [a, b ,c] is a any subarray, to check if it in non-increasing, we need to mainly check the values for nonIncreasing[0] and nonIncreasing[1]. Checking nonIncreasing[2] is not required.  

For example: [11 1 1] is non-increasing as 11 -> 1 and 1 -> 1 are non-increasing.  

So, the range of index we are checking is: [1, 2]  

Another imporant observation is that the sum of 1s in the nonIncreasing arrys will be equal to the number of elements in this array. For this example, range of index is [1, 2] and the sum of 1s in nonIncreasing array is 2. This way, we can find out if the numbers between a certain range i to j is non-increasing. We can do the same thing for non-decreasing as well.  

So make the calculation of sum of 1s optimum, we can make a prefix sum array for it.  

```java
class Solution {
    
    public List<Integer> goodIndices(int[] nums, int k) {

        int n = nums.length;

        int[] nonIncreasing = new int[n];
        int[] nonDecreasing = new int[n];

        //non-increasing
        for(int i=0; i<=n-2; i++){
            if(nums[i+1] <= nums[i]){
                nonIncreasing[i] = 1;
            }
        }

        //non-decreasing
        for(int i=0; i<=n-2; i++){
            if(nums[i+1] >= nums[i]){
                nonDecreasing[i] = 1;
            }
        }

        //create prefix array for nonIncreasing and nonDecreasing array
        int[] pfl = new int[n];
        int[] pfr = new int[n];

        pfl[0] = nonIncreasing[0];
        for(int i=1; i<n; i++){
            pfl[i] = pfl[i-1] + nonIncreasing[i];
        }

        pfr[0] = nonDecreasing[0];
        for(int i=1; i<n; i++){
            pfr[i] = pfr[i-1] + nonDecreasing[i];
        }

        List<Integer> goodIndex = new ArrayList<>();

        //edge case
        if(k == 1){
            for(int i=1; i<n-1; i++){
                goodIndex.add(i);
            }
            return goodIndex;
        }

        //we need k elements before and after index i
        for(int i=k; i<=n-k-1; i++){

            if(isGoodIndex(i-k, i-2, pfl) && isGoodIndex(i+1, i+k-1, pfr)){
                goodIndex.add(i);
            }

        }

        return goodIndex;
        
    }

    public boolean isGoodIndex(int start, int end, int[] pf){

        int sum = rangeSum(start, end, pf);
        int expectedSum = end - start + 1;

        if(sum == expectedSum) return true;
        else return false;

    }

    public int rangeSum(int L, int R, int[] pf){
        if(L == 0)
            return pf[R];
        else
            return pf[R] - pf[L-1];
    }

}
```
