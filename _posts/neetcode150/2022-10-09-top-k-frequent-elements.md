---
layout: post
title: Top K Frequent Elements
date: 2022-10-09 15:23 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## Problem Description

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.  
[leetcode](https://leetcode.com/problems/top-k-frequent-elements/)

### Solution

### APPROACH 1

```java
class Solution {

    public int[] topKFrequent(int[] nums, int k) {

        Map<Integer, Integer> map = new HashMap<>();

        int max = 0;

        for(int i=0; i<nums.length; i++){

            int x = nums[i];

            if(map.containsKey(x)){
                map.put(x, map.get(x) + 1);
            }else{
                map.put(x, 1);
            }

            max = Math.max(max, map.get(x));

        }

        Map<Integer, List<Integer>> m2 = new HashMap<>();

        for(Map.Entry<Integer, Integer> entry: map.entrySet()){

            int key = entry.getKey();
            int value = entry.getValue();

            if(!m2.containsKey(value)){
                List<Integer> l = new ArrayList<>();
                l.add(key);
                m2.put(value, l);
            }else{
                m2.get(value).add(key);
            }

        }


        int[] ans = new int[k];
        int idx=0;

        while(k>0){
            if(m2.containsKey(max)){
                List<Integer> temp = m2.get(max);
                for(Integer x: temp){
                    ans[idx] = x;
                    idx++;
                    k--;
                }
            }
            max--;
        }

        return ans;

    }

}
```

### APPROACH 2 - USING HEAP

```java
class Solution {

    public int[] topKFrequent(int[] nums, int k) {

        int n = nums.length;

        // frequency map
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<n; i++){
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }

        // min heap
        // based on frequency of the elements, which we can get from the hashmap
        Queue<Integer> heap = new PriorityQueue<>( (a, b) -> map.get(a) - map.get(b) );

        // add the keys one by one to the min heap
        // the key with min freq will be at the top after each addition
        // if the size of heap is > k, remove the top most element (keep doing that to have at most k elements in the heap)
        // at the end, we will have k elements in the heap which have the highest freq
        for(Integer x: map.keySet()){
            heap.add(x);
            if(heap.size() > k) heap.poll();
        }

        // get k values from the heap and store it in the result array
        int[] res = new int[k];
        for(int i=0; i<k; i++){
            res[i] = heap.poll();
        }

        return res;

    }

}
```

This can be solved using max heap also, but we will need to store all n elements in the heap first. So the space complexity will be higher. Using min heap, we can maintain the maximum heap size as k elements, which helps in optimizing the space.
