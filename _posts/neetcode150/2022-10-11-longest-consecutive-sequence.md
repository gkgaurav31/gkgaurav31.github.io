---
layout: post
title: Longest Consecutive Sequence
date: 2022-10-11 00:39 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.
You must write an algorithm that runs in O(n) time.
[leetcode](https://leetcode.com/problems/longest-consecutive-sequence/)

### Solution

The brute force solution for this problem can be so loop through every element in the array and track the max length/streak possible.
We can do better in terms of time complexity by first sorting the array.
However, there is one key observation which can make the solution optimal. An element X can be the beginning of a streak only if the (X-1) is not present in the array. To make this lookup better, we can use HashSet. Only when we get any element which can be the start of a streak, we will continue our count.  

```java
class Solution {
    
    public int longestConsecutive(int[] nums) {
        
        Set<Integer> set = new HashSet<>();
        
        for(int i=0; i<nums.length; i++){
            set.add(nums[i]);
        }
        
        int maxLength=0;
        
        for(Integer x: set){
            
            //x can be the start of a streak as (x-1) is not in the HashSet
            if(!set.contains(x-1)){
                
                int currentLength=1;
                
                while(set.contains(x+1)){
                    currentLength++;
                    x++;
                }
                
                maxLength = Math.max(currentLength, maxLength);
                
            }
            
        }
        
        return maxLength;
        
    }
}
```
