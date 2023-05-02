---
layout: post
title: First Missing Positive
date: 2023-05-02 23:33 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an unsorted integer array nums, return the smallest missing positive integer.
You must implement an algorithm that runs in O(n) time and uses constant extra space.

[leetcode](https://leetcode.com/problems/first-missing-positive/description/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/first_missing_positive.png)

```java
class Solution {

    public int firstMissingPositive(int[] nums) {

        int n = nums.length;

        //replace all -ve numbers and 0 with a number which cannot be the answer, like N+2
        for(int i=0; i<n; i++){
            if(nums[i] <= 0) nums[i] = n+2;
        }

        //mark the numbers which are present by making the corresponding index negative
        //For example, if nums[i] is 5, then make number at index 4 negative
        for(int i=0; i<n; i++){

            //important to take the absolute here, because we might have change this value to negative before
            int currentNum = Math.abs(nums[i]);
            
            //if the answer is in range, change the required index to negative
            if(currentNum >= 1 && currentNum <= n){
                nums[currentNum-1] = -1 * Math.abs(nums[currentNum-1]);
            }

        }

        //check first non-negative index which will be our answer if anything is missing
        for(int i=0; i<n; i++){
            if(nums[i] > 0){
                return i+1;
            }
        }

        //otherwise, return N+1
        //Example: [1, 2, 3, 4] => First missing = n+1 = 5
        return n+1;

    }

}
```
