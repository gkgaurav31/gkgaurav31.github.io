---
layout: post
title: Kth Largest Element in an Array
date: 2022-10-15 23:39 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer array nums and an integer k, return the kth largest element in the array.
Note that it is the kth largest element in the sorted order, not the kth distinct element.
You must solve it in O(n) time complexity.
[leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### Solution

The trivial way is to sort the list and return the element at K-1 index.  

We can also make use of Max Heap which is going to O(N + KlogN) time complexity.

```java
class Solution {
    
    public int findKthLargest(int[] nums, int k) {
        
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>(Collections.reverseOrder());
        
        for(int i=0; i<nums.length; i++){
            pq.add(nums[i]);
        }
        
        int ans = 0;
        
        for(int i=1; i<k; i++){
            pq.poll();
        }
        
        return pq.poll();
        
    }
    
}
```
