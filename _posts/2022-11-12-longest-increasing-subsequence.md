---
layout: post
title: Longest Increasing Subsequence
date: 2022-11-12 14:27 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## Problem Description

Given an integer array nums, return the length of the longest strictly increasing subsequence  

[leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/longest_increasing_subsequence.png)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        
        int n = nums.length;

        int[] dp = new int[n];

        //The subsequence can be a single element, so the min possible answer is 1
        Arrays.fill(dp, 1);

        //Find LIS after include next element one by one
        for(int i=1; i<n; i++){
            
            //Loop through all the previous index, that is: 0 to i-1
            //If the element at current position is greater than element at j, we can include the current number
            //The LIS for [0-j] is stored at dp[j]
            //So, dp[i] is max of (dp[i], dp[j] + 1)
            for(int j=i-1; j>=0; j--){
                if(nums[i] > nums[j]){
                    dp[i] = Math.max(dp[i], dp[j] + 1);

                }
            }

        }

        int max=1;

        for(int i=0; i<n; i++){
            max = Math.max(max, dp[i]); 
        }

        return max;

    }
}
```

The above algorithm is O(N^2) time complexity. We can further optimize the solution by using DP + Binary Search.
