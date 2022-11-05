---
layout: post
title: Longest Increasing Subsequence
date: 2022-11-05 23:52 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given an integer array nums, return the length of the longest strictly increasing subsequence  

[leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/)  

### Solution

```java
class Solution {

    public int lengthOfLIS(int[] nums) {
        
        int n = nums.length;

        int[] dp = new int[n];

        Arrays.fill(dp, 1);

        for(int i=0;i<n;i++){
            for(int j=0; j<i; j++){
                if(nums[i]>nums[j]){
                    dp[i]=Math.max(dp[i], dp[j]+1);
                }
            }
        }

        int max=1;
        for(int i=0; i<n; i++) max = Math.max(max, dp[i]);

        return max;
    }

}
```
