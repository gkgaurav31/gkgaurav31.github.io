---
layout: post
title: Trapping Rain Water
date: 2022-07-02 00:35 +0530
author: "Gaurav Kumar"
tags: "java arrays arrays neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/trapping_rain_water.jpg)

We would need to think of how we can calculate the water which can be trapped at each block. Let's say that the max height on the left side of current block is maxL and max height on the right side of the current block is maxR, then the water which can be trapped will be: min(maxL, maxR) - currentHeight. This will be true for most cases, but this will be negative if the currentHeight is greater than than both maxL and maxR. In that case, the water trapped will be 0. Also, for the left most and right most block, there will be no water trapped. We can maintain the maxL and maxR for any block using prefix array.

```java
class Solution {
    public int trap(int[] height) {
        
        int n = height.length;

        int[] pfl = new int[n]; //prefix array to track max before i index
        int[] pfr = new int[n]; //prefix array to track max after i index

        pfl[0] = 0;
        int currentMax = 0;
        for(int i=1; i<n; i++){
            if(height[i-1] > currentMax) currentMax = height[i-1];
            pfl[i] = Math.max(pfl[i-1], currentMax);
        }

        pfr[n-1] = 0;
        currentMax=0;
        for(int i=n-2; i>=0; i--){
            if(height[i+1] > currentMax) currentMax = height[i+1];
            pfr[i] = Math.max(pfr[i+1], currentMax);
        }

        int total=0;

        for(int i=1; i<n-1; i++){
            total += Math.max(0, Math.min(pfl[i],pfr[i])-height[i]); //If the currentHeight is more than the max on left and right both, take 0.
        }

        return total;
    }
}
```
