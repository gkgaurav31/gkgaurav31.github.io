---
layout: post
title: Kth Largest Element in a Stream
date: 2022-10-19 20:06 +0530
author: "Gaurav Kumar"
tags: "java heap leetcode important"
categories: "heap"
---

## Problem Description

Design a class to find the kth largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Implement KthLargest class:

- KthLargest(int k, int[] nums) Initializes the object with the integer k and the stream of integers nums.
- int add(int val) Appends the integer val to the stream and returns the element representing the kth largest element in the stream.

[leetcode](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

### Solution

```java
class KthLargest {
    
    PriorityQueue<Integer> pq;
    int size;

    public KthLargest(int k, int[] nums) {
        
        this.size=k;
        
        pq = new PriorityQueue<>();
        
        for(int i=0; i<nums.length; i++) pq.add(nums[i]);
        
        while(pq.size() > k) pq.poll();
        
    }
    
    public int add(int val) {
        
        pq.add(val);
        
        if(pq.size() > size)
            pq.poll();
        
        return pq.peek();
        
    }
}
```
