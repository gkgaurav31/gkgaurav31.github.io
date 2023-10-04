---
layout: post
title: Missing Ranges
date: 2023-10-04 19:39 +0530
author: "Gaurav Kumar"
tags: "java matrix leetcode leetcodealgo100"
categories: "matrix"
---

## PROBLEM DESCRIPTION

You are given an inclusive range [lower, upper] and a sorted unique integer array nums, where all elements are within the inclusive range.

A number x is considered missing if x is in the range [lower, upper] and x is not in nums.

Return the shortest sorted list of ranges that exactly covers all the missing numbers. That is, no element of nums is included in any of the ranges, and each missing number is covered by one of the ranges.

[leetcode](https://leetcode.com/problems/missing-ranges/)

## SOLUTION

### APPROACH 1

```java
class Solution {

    public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper) {

        int n = nums.length;

        List<List<Integer>> result = new ArrayList<>();

        // edge case
        // If the nums array is empty, the entire [lower, upper] range is missing.
        if (n == 0) {
            result.add(Arrays.asList(lower, upper));
            return result;
        }

        //init: start and end of a range which is missing
        int start = -1;
        int end = -1;

        //idx will be used while iterating the nums array
        int idx = 0;

        // Iterate through the range [lower, upper] and nums array simultaneously.
        for (int i = lower; i <= upper && idx < n; i++) {

            // If the current number in nums does not match the current value in the range,
            // mark the start and end of a missing range.
            if (nums[idx] != i) {

                start = i;
                end = i;

                //j: to find the end of the range
                int j = i;

                // Continue checking for consecutive missing numbers.
                while (nums[idx] != j) {
                    end = j;
                    j++;
                }

                // Add the missing range to the result.
                result.add(Arrays.asList(start, end));

                // Update i to the last missing number in the range.
                i = end;

            } else {
                // If the current number in nums matches the current value in the range,
                // move to the next number in nums.
                idx++;
            }

        }

        // If there are missing numbers after the last number in nums, add a missing range.
        if (n > 0 && nums[n - 1] < upper) {
            result.add(Arrays.asList(nums[n - 1] + 1, upper));
        }

        return result;
    }
}
```

### OPTIMIZED

The main observersation is that if num[i-1] - nums[i] is > 1, there must be a missing range. That missing range will be: `nums[i-1]+1, nums[i]-1`. There are three other important edge cases which need to be handled:

- A missing range before the first element of the nums array.
- A missing range after the last element of the nums array.
- nums array is empty, in which case we can return the range [lower, upper] directly as the answer

```java
class Solution {

    public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper) {

        int n = nums.length;

        List<List<Integer>> res = new ArrayList<>();

        // If the nums array is empty, the entire [lower, upper] range is missing.
        if(n == 0){
            res.add(Arrays.asList(lower, upper));
            return res;
        }

        // If the first number in nums is greater than the lower bound,
        // add a missing range from lower to nums[0] - 1.
        if(nums[0] > lower){
            res.add(Arrays.asList(lower, nums[0] - 1));
        }

        // Iterate through the sorted nums array to find and record missing ranges.
        for(int i = 1; i < n; i++){
            if(nums[i] - nums[i - 1] > 1){
                // If there's a gap between consecutive numbers in nums, add a missing range.
                res.add(Arrays.asList(nums[i - 1] + 1, nums[i] - 1));
            }
        }

        // If the last number in nums is less than the upper bound,
        // add a missing range from nums[n-1] + 1 to upper.
        if(nums[n - 1] < upper){
            res.add(Arrays.asList(nums[n - 1] + 1, upper));
        }

        return res;
    }
}
```
