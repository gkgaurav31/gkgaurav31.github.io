---
layout: post
title: Summary Ranges
date: 2023-08-03 20:51 +0530
author: "Gaurav Kumar"
tags: "java intervals leetcode leetcode150"
categories: "intervals"
---

## PROBLEM DESCRIPTION

You are given a sorted unique integer array nums.

A range [a,b] is the set of all integers from a to b (inclusive).

Return the smallest sorted list of ranges that cover all the numbers in the array exactly. That is, each element of nums is covered by exactly one of the ranges, and there is no integer x such that x is in one of the ranges but not in nums.

Each range [a,b] in the list should be output as:

```text
"a->b" if a != b
"a" if a == b
```

[leetcode](https://leetcode.com/problems/summary-ranges/)

## SOLUTION

```java
class Solution {
    
    public List<String> summaryRanges(int[] nums) {

        int n = nums.length;
        
        List<String> list = new ArrayList<>(); 

        for(int i=0; i<n; i++){

            // Initialize the start of the current range.
            int start = nums[i]; 

            // While the next number is consecutive, continue to the next element.
            while(i+1 < n && nums[i+1]-nums[i] == 1){
                i++;
            }

            // Check if the range contains only one element or more.
            // The array contains unique numbers in sorted order already, so no duplicates
            // So, if the current number is same as we have taken for the start, it means that there is only a single number for this range
            if(nums[i] == start){
                list.add(String.valueOf(start)); // If only one element, add it as "a".
            }else{
                list.add(start + "->" + nums[i]); // If more than one element, add it as "a->b".
            }

        }

        return list; 

    }

}
```
