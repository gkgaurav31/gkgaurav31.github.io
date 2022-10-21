---
layout: post
title: Sort K-Sorted Array
date: 2022-10-22 01:04 +0530
author: "Gaurav Kumar"
tags: "java heap geeksforgeeks important"
categories: "heap"
---

## Problem Description

Given an array of n elements, where each element is at most k away from its target position, you need to sort the array optimally.  

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/nearly-sorted-1587115620/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

### Solution

The problem mentions that the array is k-sorted, which means that any element is at most K positions away from it's correct position in the sorted array. So, if we loop at the first K+1 elements and find minimum among them, we can place at in the beginning of this "window". To optimizing finding the min element, we can make use of min heap.

- Create a min heap of k elements
- Start iterating from (k+1)th element. Add the current element to min heap.
- Get the min element from min heap and insert it into answer list
- Move to the next element and repeat this process
- Once we reach end of array, we will come out of the loop.
- The remaining elements in the min heap can be added to the answer list.

__Edge Case:__ If k is more than n, we can consider k=n to avoid IndexOutOfBound.

```java
class Solution
{

    ArrayList <Integer> nearlySorted(int arr[], int num, int k)
    {

        int n = arr.length;
    
        if(k>n) k=n;
    
        ArrayList<Integer> ans = new ArrayList<>();
    
        PriorityQueue<Integer> pq = new PriorityQueue<>();
    
        for(int i=0; i<k; i++){
          pq.add(arr[i]);
        }
    
        int end = k;
        
        while(end<arr.length){
          pq.add(arr[end]);
          ans.add(pq.poll());
          end++;
        }
    
        while(pq.size() != 0) ans.add(pq.poll());
        
        return ans;
        
    }
}
```
