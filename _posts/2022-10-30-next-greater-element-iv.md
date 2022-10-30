---
layout: post
title: Next Greater Element IV
date: 2022-10-30 17:33 +0530
author: "Gaurav Kumar"
tags: "java stack important"
categories: "stack"
---

## Problem Description

You are given a 0-indexed array of non-negative integers nums. For each integer in nums, you must find its respective second greater integer.  
The second greater integer of nums[i] is nums[j] such that:  

- j > i  
- nums[j] > nums[i]  
- There exists exactly one index k such that nums[k] > nums[i] and i < k < j.  

If there is no such nums[j], the second greater integer is considered to be -1.  

Return an integer array answer, where answer[i] is the second greater integer of nums[i].  

[leetcode](https://leetcode.com/problems/next-greater-element-iv/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/second_greater.png)

```java
class Solution {
    
    public int[] secondGreaterElement(int[] nums) {
        
        int n = nums.length;
        
        //nextGreater
        int nextGreater[] = new int[n];
        
        Stack<Integer> stack = new Stack<>();
        stack.push(n-1);
        
        //create nextGreater array (based on index) 
        for(int i=n-1; i>=0; i--){
            
            int current = i;
            
            while(!stack.isEmpty() && nums[current] >= nums[stack.peek()]){
                stack.pop();
            }
            
            if(stack.isEmpty()){
                nextGreater[i] = -1;
                stack.push(current);
            }else{
                nextGreater[i] = stack.peek();
                stack.push(current);
            }
            
        }
        
        int[] secondGreater = new int[n];
        for(int i=0; i<n; i++) secondGreater[i] = -1; //initialize
        
        for(int i=0; i<n; i++){
            
            if(nextGreater[i] == -1){
                secondGreater[i] = -1; continue;
            }
            
            //second greater will be after first greater element
            int j = nextGreater[i] + 1; 
            
            //look for second greater from j to n-1
            //if the element we are checking is less than arr[i], we directly move to next greater element of arr[j]
            while(j<n && j!=-1){
                if(nums[j] > nums[i]){
                    secondGreater[i] = nums[j];
                    break;
                }else{
                    j = nextGreater[j];
                }
            }
            
        }
        
        return secondGreater;
        
    }
    
}
```
