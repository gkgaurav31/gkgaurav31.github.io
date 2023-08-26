---
layout: post
title: Maximum Sum Circular Subarray
date: 2023-08-26 21:45 +0530
author: "Gaurav Kumar"
tags: "java arrays kadane leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray of nums.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of nums[i] is nums[(i + 1) % n] and the previous element of nums[i] is nums[(i - 1 + n) % n].

A subarray may only include each element of the fixed buffer nums at most once. Formally, for a subarray nums[i], nums[i + 1], ..., nums[j], there does not exist i <= k1, k2 <= j with k1 % n == k2 % n.

[leetcode](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=fxT9KjakYPM)

The classic Kadane's algorithm works effectively in finding the maximum sum within a regular array. However, this problem presents an interesting twist due to the circular nature of the array.

Here's the key insight: if we identify the minimum possible sum within the array and subtract it from the total sum, we get a potential solution. This approach indirectly accounts for the circular wrapping, as the minimum sum might involve elements from the middle of the array. Alternatively, we can compute the maximum sum using a straightforward Kadane's algorithm without wrapping. By comparing these two potential answers, we can determine the maximum circular subarray sum.

It's worth noting a special case where all array numbers are negative. In this scenario, checking if the total sum equals the minimum sum can guide us. As the problem rules out an empty selection summing to 0, we would then consider the maximum sum instead. This maximum sum corresponds to the most negative number in the array since they are all negative.

```java
class Solution {

    public int maxSubarraySumCircular(int[] nums) {

        int minSum = nums[0];
        int currentMin = 0;

        int maxSum = nums[0];
        int currentMax = 0;

        int totalSum = 0;

        //we will use Kadane's algo for calculating both min and max sum
        for(int i=0; i<nums.length; i++){

            currentMax = Math.max(currentMax+nums[i], nums[i]);
            maxSum = Math.max(maxSum, currentMax);

            currentMin = Math.min(currentMin+nums[i], nums[i]);
            minSum = Math.min(minSum, currentMin);

            totalSum += nums[i];

        }

        //IMPORTANT EDGE CASE: If all the numbers are -ve
        //Example: [-3, -2, -1]
        //total sum = -6
        //min sum = -6
        //totalsum - minsum = 0 (this will be the case when we do not take any element from the array)
        //the problem states that we cannot have an empty pick, so the answer would then be the maxSum in that array
        if(totalSum == minSum)
            return maxSum;

        return Math.max(totalSum-minSum, maxSum);

    }

}
```
