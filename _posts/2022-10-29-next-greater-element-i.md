---
layout: post
title: Next Greater Element I
date: 2022-10-29 23:28 +0530
author: "Gaurav Kumar"
tags: "java search"
categories: "search"
---

## Problem Description

The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.  
You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.  

For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.  

Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.

[leetcode](https://leetcode.com/problems/next-greater-element-i/)

### Solution

For each element in nums1, we need to find the next greater element (on right side) as per the array nums2.  

We can use a stack to find the next greater element in the nums2. It would make sense to start checking from the right side of the array. The last element will have no larger element so we can simply push that in the stack and the answer will be -1 for it. We will keep storing our answer in a HashMap.  

If the next element is greater than the top of stack, we can keep popping it from the stack. We will keep checking this condition with the top of the stack. If the element if less than the top of the stack, we have got the next largest element for that element which will be the top of the stack. Add it to the hashmap. The current element can be the answer for subsequent elements that we will be checking, so we push it to the stack.

```java
class Solution {
    
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        
        int n = nums2.length;
        
        Stack<Integer> stack = new Stack<>();
        stack.push(nums2[n-1]);
        
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(nums2[n-1], -1);        
        
        for(int i=n-2; i>=0; i--){
            
            int current = nums2[i];
            
            while(!stack.isEmpty() && current > stack.peek()){
                stack.pop();
            }
            
            if(stack.isEmpty()){
                map.put(current, -1);
            }else{
                map.put(current, stack.peek());
            }
            
            stack.push(current);
            
        }
        
        int ans[] = new int[nums1.length];
        for(int i=0; i<nums1.length; i++){
            ans[i] = map.get(nums1[i]);
        }
        
        return ans;
    }
    
}
```
