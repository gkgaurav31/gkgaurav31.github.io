---
layout: post
title: Partition Equal Subset Sum
date: 2022-11-20 14:18 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

[leetcode](https://leetcode.com/problems/partition-equal-subset-sum/description/)  

### Solution

[NeetCode Youtube](https://www.youtube.com/watch?v=IsvocB5BJhw)

```java
class Solution {
    
    public boolean canPartition(int[] nums) {
        
        Set<Integer> set = new HashSet<>();

        set.add(0);

        int n = nums.length;
        int sum = 0;
        for(int i=0; i<n; i++) sum += nums[i];

        if(sum%2 == 1) return false;

        int k = sum/2;

        for(int i=0; i<n; i++){

            Set<Integer> tempSet = new HashSet<>();

            for(Integer x: set){
                tempSet.add(x+nums[i]);
            }

            set.addAll(tempSet);

            if(set.contains(k)) return true;

        }

        return false;

    }
}
```
