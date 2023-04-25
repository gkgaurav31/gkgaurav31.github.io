---
layout: post
title: Majority Element
date: 2023-04-25 23:41 +0530
tags: "java arrays leetcode important"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

[leetcode](https://leetcode.com/problems/majority-element/description/)

### SOLUTION

[Boyer-Moore Majority Voting Algorithm](https://www.geeksforgeeks.org/boyer-moore-majority-voting-algorithm/)

The basic idea is that if we cancel two distinct elements, then the majority element does not change.

To find the majority element, we can keep "cancelling" two distinct elements. The last element left with >= 1 freqency can be the majorly element (not certain, if there are no majority elements). So, we check the frequency of the element we are left with at the end to check if its a majority element.

```java
class Solution {

    public int majorityElement(int[] nums) {

        //init with first element
        int element = nums[0];
        int frequency = 1;

        //loop through other elements one by one
        for(int i=1; i<nums.length; i++){

            //if the previous two elements were distinct, they would have cancelled each other and the frequency will be 0
            //in that case, set the current element as the new element with frequency 1 and compare it with others
            if(frequency == 0){
                element = nums[i];
                frequency = 1;
            //if the frequency is not 0 and the current element does not match with arr[i], we can cancel one pair
            //to do this, we can simply reduce the frequency of current element by 1  
            }else if(element != nums[i]){
                frequency--;
            //if the element matches, increase its frequency
            }else{
                frequency++;
            }
        }
        
        //we still need to check if its a majority element in case there is no majority element in the array
        //Example: 
        //1 2 1 2 1 2 3
        //at the end, we will be left with: element=3, frequency=1. However, that is not the majority element
        int c = 0;
        for(int i=0; i<nums.length; i++){
            if(nums[i] == element) c++;
        }

        return c > (nums.length/2)?element:-1;

    }

}
```
