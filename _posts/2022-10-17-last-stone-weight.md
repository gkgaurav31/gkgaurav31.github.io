---
layout: post
title: Last Stone Weight
date: 2022-10-17 00:54 +0530
author: "Gaurav Kumar"
tags: "java heap leetcode"
categories: "heap"
---

## Problem Description

You are given an array of integers stones where stones[i] is the weight of the ith stone.  

We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights x and y with x <= y. The result of this smash is:  

- If x == y, both stones are destroyed, and
- If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.

At the end of the game, there is at most one stone left.  

Return the weight of the last remaining stone. If there are no stones left, return 0.  

[leetcode](https://leetcode.com/problems/last-stone-weight/)

### Solution

```java
class Solution {
    
    public int lastStoneWeight(int[] stones) {
        
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        
        for(int i=0; i<stones.length; i++) pq.add(stones[i]);
        
        while(pq.size() > 1){
            int x = pq.poll();
            int y = pq.poll();
            
            if(x > y){
                pq.add(x-y);
            }
        }
        
        if(pq.size() == 0) return 0; //Edge case. Possible when there are only two stones which are equal in size
        
        return pq.poll();        
        
    }
    
}
```
