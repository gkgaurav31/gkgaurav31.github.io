---
layout: post
title: Target Sum
date: 2023-01-20 23:10 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given an integer array nums and an integer target.
You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

- For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

[leetcode](https://leetcode.com/problems/target-sum/description/)

### SOLUTION

```java
class Solution {

    public int findTargetSumWays(int[] nums, int target) {
        return waysHelper(nums, target, 0, new HashMap<String, Integer>(), 0);
    }

    public int waysHelper(int[] nums, int target, int idx, Map<String, Integer> map, int total){

        if(idx == nums.length){
            if(total == target) return 1;
            return 0;
        }

        if(!map.containsKey(idx+":"+total)){

            int x = waysHelper(nums, target, idx+1, map, total + nums[idx]);
            int y = waysHelper(nums, target, idx+1, map, total - nums[idx]);
            
            int t = x + y;
            
            map.put(idx+":"+total, t);

        }

        return map.get(idx+":"+total);

    }


}
```
