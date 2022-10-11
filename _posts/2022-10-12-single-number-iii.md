---
layout: post
title: Single Number III
date: 2022-10-12 00:11 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation important"
categories: "bit_manipulation"
---

## Problem Description

Given an integer array nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in any order.
You must write an algorithm that runs in linear runtime complexity and uses only constant extra space.  
[leetcode](https://leetcode.com/problems/single-number-iii/)

### Solution

Solving this problem using HashMap is quite straight forward. Without using extra space, we need to find another way.

Let's say, that the two unique elements we found were 11 and 17.  

11: 01011  
17: 10001  

11^17: 11010  

If we did a XOR on all the elements in the array, we would be left with: 11^17 = 26. We cannot directly find which two elements have have the XOR as 26. (It's like asking which two numbers have the sum X).  

One important observation:  
We have the XOR of the two elements, in which certain bits are set. That bit can be set only if the bits in that position for the two numbers are not the same.  

For example, we see that the first bit in 11^17 is set: 110**1**0.  
So, the first bit in one of the numbers must be set, and for the other one it must be unset.  

We can use this idea to solve the problem. Let's say, we create two logical groups - one with the elements whose bits are set in position 1. The other group, whose bits are not set in position 1. We loop through all the elements and put them based in this grouping. This way, we will be sure that 11 and 17 are in different group. There's another magical thing that happens. We know other elements are repeating twice. So, if there were two seves, for example, they would land up in the same group. To get our final answer, all we need to do it, take XOR of elements in group 1. Take XOR of elements in group 2. Each of them will give us the two unique numbers!

```java
class Solution {
    
    public int[] singleNumber(int[] nums) {
        
        int val = 0; //This will finally have XOR of two unique elements
        for(int i=0; i<nums.length; i++) val ^= nums[i];
        
        int pos=0;
        //Get the position of first set bit in val (xor of the two unique elements)
        for(int i=0; i<32; i++){
            if(isSet(val, i)){
                pos = i;
                break;
            }
        }
        
        int num1 = 0; //group 1 -- bit at position pos is set
        int num2 = 0; //group 2 -- bit at position pos is unset 
        
        for(int i=0; i<nums.length; i++){

            //If the bit at position pos is set, XOR it to num1
            if(isSet(nums[i], pos)){
                num1 ^= nums[i];
            //Else, XOR it to num2
            }else{
                num2 ^= nums[i];
            }

            //Duplicates will land up in the same "group" and effectively become 0 (because a^a is 0)
            //So, finally we will be left with the unique non-repeating elements in each group
        }
        
        return new int[]{num1, num2};
        
    }
    
    public boolean isSet(int val, int pos){
        val = val>>pos;
        return (val&1)==1;
    }
}
```
