---
layout: post
title: Container With Most Water
date: 2022-10-22 21:43 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).  

Find two lines that together with the x-axis form a container, such that the container contains the most water.  

Return the maximum amount of water a container can store.  

Notice that you may not slant the container.  

[leetcode](https://leetcode.com/problems/container-with-most-water/)

### Solution

### Two Pointer Approach

```java
class Solution {
    public int maxArea(int[] height) {
        
        int max = 0;
        
        int i=0;
        int j=height.length-1;
        
        while(i<j){
            
            int amount =(j-i)*(Math.min(height[i], height[j]));
            max = Math.max(max, amount);
            
            if(height[i] > height[j]){
                j--;
            }else{
                i++;
            }
            
        }
        
        return max;
        
    }
}
```
