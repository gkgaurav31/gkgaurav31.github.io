---
layout: post
title: Swapping pairs make sum equal (geeksforgeeks - SDE Sheet)
date: 2024-08-25 17:12 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two arrays of integers `a[]` and `b[]` of size `n` and `m`, the task is to check if a pair of values (one value from each array) exists such that swapping the elements of the pair will make the sum of two arrays equal.

Note: Return `1` if there exists any such pair otherwise return `-1`.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/swapping-pairs-make-sum-equal4142/1?page=2)

## SOLUTION

```java
class Solution {

    long findSwapValues(long a[], int n, long b[], int m) {

        long sum1 = getSum(a);
        long sum2 = getSum(b);

        long diff = sum1 - sum2;

        // let's say we are looking for x in arr1 and y in arr2
        // what we need is:
        // sum1 - x + y = sum2 - y + x
        // => sum1 - sum2 = 2 * (x - y)
        // => diff = 2 * (x - y)

        // if diff is odd, no pair will be possible
        if(diff%2 != 0)
            return -1;

        // store all numbers in 2nd array in a hashset for quick lookup
        Set<Long> set = new HashSet<>();
        for(int i=0; i<m; i++)
            set.add(b[i]);

        // iterate over the first array
        for(int i=0; i<n; i++){

            // assume current num to x
            long x = a[i];

            // diff = 2x - 2y
            // 2y = 2x - diff
            // y = (2x - diff)/2

            // we can determine value of y based on x and diff
            long y = x - diff/2;

            // if y is present in second array, return 1
            if(set.contains(y))
                return 1;

        }

        return -1;

    }

    long getSum(long[] arr){

        long sum = 0;
        for(int i=0; i<arr.length; i++)
            sum += arr[i];
        return sum;

    }

}
```
