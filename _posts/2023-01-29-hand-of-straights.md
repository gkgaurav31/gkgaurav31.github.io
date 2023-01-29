---
layout: post
title: Hand of Straights
date: 2023-01-29 12:35 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size groupSize, and consists of groupSize consecutive cards.

Given an integer array hand where hand[i] is the value written on the ith card and an integer groupSize, return true if she can rearrange the cards, or false otherwise.

[leetcode](https://leetcode.com/problems/hand-of-straights/description/)

### SOLUTION

One way to solve this problem is by making use of minHeap to get the starting point for group, and then keep remove elements from the heap based on groupSize. If the next element which needs to be present in that group is not present, return false.

```java
class Solution {

    public boolean isNStraightHand(int[] hand, int groupSize) {

        int n = hand.length;

        //the number of cards needs to be a multiple of group size
        if(n%groupSize != 0) return false;

        //min heap -> to get the start number for the current group
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i=0; i<n; i++) pq.add(hand[i]);

        //while min heap is not empty
        while(!pq.isEmpty()){

            //the start number must be the minimum element
            int startNum = pq.poll();

            //we need more (groupsize -1) cards to form the group
            //the first number in that group is "startNum"
            //we can find the rest by adding +1
            for(int i=1; i<=groupSize-1; i++){
                
                //the next number we need in that group 
                int nextNum = startNum + i;

                //if min heap does not have this number, return false
                if(!pq.contains(nextNum)) return false;

                //otherwise, remove that number from min heap and continue checking for rest
                pq.remove(nextNum);

            }

        }

        return true;

    }

}
```
