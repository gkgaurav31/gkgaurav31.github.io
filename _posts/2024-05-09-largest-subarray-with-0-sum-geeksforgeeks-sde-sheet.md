---
layout: post
title: Largest subarray with 0 sum (geeksforgeeks - SDE Sheet)
date: 2024-05-09 20:16 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks queue important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array having both positive and negative integers. The task is to compute the length of the largest subarray with sum 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1?page=1)

## SOLUTION

We will keep calculating the `currentSum` while iterating from the elements of the array and store it in a `HashMap`. The key will be the sum and value will be the index till which we have got this sum.

The main concept here is -

If we get a sum `x` after adding the current element, and we see that this `currentSum` is already present in the `HashMap`, then we have got a subarray which has a sum of 0.

> |----------x---------|------y------|  
> If x + y is still x, then y must be 0.

We have mainly three conditions:

- If the current element itself is `0`. If the `maxLength` till then is `0`, then update it to `1`.
- If the `currentSum` itself becomes `0`, it has to be the largest length from the 0th index. So update `maxLength` to `i+1`.
- If the `currentSum` is something which was already seen before. Then, get the index of previous instance when we got this sum and calculate the length from that index to the current index.

**IMPORTANT:** When updating the HashMap with `currentSum` and its index, if we have got a sum which was already present, then we should keep the minimum previous index since that will give us larger length.

```java
class GfG
{
    int maxLen(int arr[], int n)
    {

        // track max length
        int maxLength = 0;

        // track current sum
        int currentSum = 0;

        // store sum and index for that sum
        Map<Integer, Integer> map = new HashMap<>(); // [sum, index]

        // loop through all elements
        for(int i=0; i<n; i++){

            // add to current sum
            currentSum += arr[i];

            // case 1: current element is 0 and maxlength is also 0
            if(arr[i] == 0 && maxLength == 0)
                maxLength = 1;

            // case 2: currentSum is 0, update maxLength to i+1
            if(currentSum == 0)
                maxLength = i+1;

            // case 3: if the currentSum was seen before
            // then the elements from previous occurance till current index must sum up to 0
            // using the index of previous occurance, calculate the length of this sub array and update the maxLength
            if(map.containsKey(currentSum))
                maxLength = Math.max(maxLength, i - map.get(currentSum));

            // IMPORTANT:
            // during case 3, we must preserve the lower index in the hashmap since that will give us larger subarray
            map.put(currentSum, Math.min(i, map.getOrDefault(currentSum, i)));

        }

        return maxLength;

    }
}
```
