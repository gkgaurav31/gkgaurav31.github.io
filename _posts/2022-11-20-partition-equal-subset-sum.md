---
layout: post
title: Partition Equal Subset Sum
date: 2022-11-20 14:18 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## Problem Description

Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

[leetcode](https://leetcode.com/problems/partition-equal-subset-sum/description/)  

### Solution

[NeetCode Youtube](https://www.youtube.com/watch?v=IsvocB5BJhw)

- Get total sum of all the elements in the nums array
- If the total sum if odd, return false, since we cannot divide it equally into two subsets.
- If it's even, our goal is to find a subset with sum = totalsum/2. Because, the sum of the other subset remaining will definitely be the same as half sum.
- We start with an empty hash set and add 0 to it because 0 is always possible: {0}
- Then, we loop through each element and include other possible sums if we take the current element. For example:  

```text
nums = [1,2,3,5]

set = {0} -> init

i=0
{0, 1}

i=1
{0, 1, 2, 3}

i=2
{0, 1, 2, 3, 4, 5, 6}

i=3
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11}
```

- If the target sum if presnet in the hashset, return true

```java
class Solution {
    
    public boolean canPartition(int[] nums) {

        int totalSum = getSum(nums);

        if(totalSum%2 == 1) return false;

        int halfSum = totalSum/2;

        Set<Integer> possibleSubsetSum = new HashSet<>();
        possibleSubsetSum.add(0);

        for(int i=0; i<nums.length; i++){

            Set<Integer> temp = new HashSet<>();

            for(Integer x: possibleSubsetSum){
                temp.add(x + nums[i]);
            }

            possibleSubsetSum.addAll(temp);

            if(possibleSubsetSum.contains(halfSum)) return true;

        }

        return false;

    }

    public int getSum(int[] nums){
        int sum = 0;
        for(int i=0; i<nums.length; i++){
            sum += nums[i];
        }
        return sum;
    }

}
```
