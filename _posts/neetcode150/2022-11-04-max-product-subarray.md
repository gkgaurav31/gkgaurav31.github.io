---
layout: post
title: Max Product Subarray
date: 2022-11-04 18:26 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150 important"
categories: "neetcode150"
---

## Problem Description

Given an integer array nums, find a subarray that has the largest product, and return the product.  
The test cases are generated so that the answer will fit in a 32-bit integer.  

[leetcode](https://leetcode.com/problems/maximum-product-subarray/description/)

### Solution

Since the array can contain negative numbers also, we need to main currentMax as well as currentMin. Because if there are even number of negative numbers in the subarray, the product will be positive.

```java
class Solution {

    public int maxProduct(int[] nums) {

        int n = nums.length;

        int currentMax=nums[0];
        int currentMin=nums[0];

        int ans = currentMax;

        for(int i=1; i<n; i++){

            //tempMax = max(currentElement or currentMax*currentElement or currentMin*currentElement)
            int tempMax = Math.max(nums[i], Math.max(currentMax*nums[i], currentMin*nums[i]));

            //tempMin = min(currentElement or currentMax*currentElement or currentMin*currentElement)
            int tempMin = Math.min(nums[i], Math.min(currentMax*nums[i], currentMin*nums[i]));
            
            currentMax = tempMax;
            currentMin = tempMin;

            ans = Math.max(ans, currentMax);

        }

        return ans;
    }
}
```
