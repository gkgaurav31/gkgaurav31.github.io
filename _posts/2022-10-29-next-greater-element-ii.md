---
layout: post
title: Next Greater Element II
date: 2022-10-29 23:42 +0530
author: "Gaurav Kumar"
tags: "java stack"
categories: "stack"
---

## Problem Description

Given a circular integer array nums (i.e., the next element of nums[nums.length - 1] is nums[0]), return the next greater number for every element in nums.  
The next greater number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return -1 for this number.  

[leetcode](https://leetcode.com/problems/next-greater-element-ii/)

### Solution

This question is based on: [Next Greater Element I]({% post_url 2022-10-29-next-greater-element-i %})
The trick is to add all elements in the stack instead of just the last elements (from right to left) because the question mentions that we can treat is as a cirular array.

```java
class Solution {

    public int[] nextGreaterElements(int[] nums) {
        
        int n = nums.length;
        int ans[] = new int[n];
        
        Stack<Integer> stack = new Stack<>();
        
        for(int i=n-1; i>=0; i--){
            stack.push(nums[i]);
        }
        
        for(int i=n-1; i>=0; i--){
            
            int current = nums[i];
            
            while(!stack.isEmpty() && current >= stack.peek()){
                stack.pop();
            }
            
            if(stack.isEmpty()){
                ans[i] = -1;
                stack.push(current);
            }else{
                ans[i] = stack.peek();
                stack.push(current);
            }
            
        }
        
        return ans;
        
    }
}
```
