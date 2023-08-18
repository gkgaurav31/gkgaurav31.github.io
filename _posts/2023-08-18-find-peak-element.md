---
layout: post
title: Find Peak Element
date: 2023-08-18 18:03 +0530
author: "Gaurav Kumar"
tags: "java binary_search leetcode leetcode150 important"
categories: "binary_search"
---

## Problem Description

A peak element is an element that is strictly greater than its neighbors.
Given a 0-indexed integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.
You may imagine that nums[-1] = nums[n] = -∞. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.
You must write an algorithm that runs in O(log n) time.

[leetcode](https://leetcode.com/problems/find-peak-element/)

## Solution

Let's take some examples to understand the intuition behind using binary search.

> array = [1, 2, 4, 6, 8, 10, 12, 14, 16, 18]

This is a monotonically increasing array. Let's say we find the mid as index 5, which is 10. Now, the peak can actually exist on both the sides. But if we carefully observe, it is certain that there is a peak on the right side of 10. There are two scenarios possibly:

- The numbers on the right keep increasing. In which case, the last index is going to be the answer. As per the problem, an element is always considered to be strictly greater than a neighbor that is outside the array.
- There is a drop in the number while going towards right. In that case too, we have a peak! (index before the drop)

> array = [20, 18, 15, 12, 10, 8, 5, 3, 1]

The same scenario will happen for a monotonically decreasing array. In that case, the element at index 0 will be the peak.

> For a general case, we would have the following options mainly:

- Both left and right numbers are more than the current number. So, current number is the peak.
- Only of the numbers on either side is greater than it. Go towards that number to get the peak.

```java
class Solution {

    public int findPeakElement(int[] nums) {

        int n = nums.length;

        int l = 0;
        int r = n-1;

        //edge case
        if(n == 1) return 0;

        //binary search
        while(l<=r){

            //get mid
            int mid = (l+r)/2;

            //if mid if 0th index
            if(mid == 0){

                //if the first number is more than its right, we can return it as the answer
                //as per the problem, 0th index element is greater than it's left side
                if(nums[mid] > nums[mid+1]){
                    return mid;
                //otherwise, update left to mid+1 as it can be the possible answer
                }else{
                    l = mid + 1;
                }

            //if mid is last index
            }else if(mid == n-1){

                //if the last element is more than the last but one element, we can return it
                if(nums[mid] > nums[mid-1]){
                    return mid;
                //otherwise, update right to mid-1 as it can be the possible answer
                }else{
                    r = mid - 1;
                }

            //if mid is some middle element
            }else{

                //if it's more than both of its neighbors, we can return it as the peak
                if(nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1]){
                    return mid;
                //otherwise, one of the neighbors must be greater than it
                //if the left one is more, we can reduce the right side of our search area
                }else if(nums[mid] < nums[mid-1]){
                    r = mid - 1;
                //otherwise, right one would be more and we can reduce the left side of our search area
                }else{
                    l = mid + 1;
                }

            }

        }

        return -1;

    }

}
```

### ANOTHER WAY TO CODE

[NeetCode YouTube](https://www.youtube.com/watch?v=kMzJy9es7Hc)

```java
class Solution {

    public int findPeakElement(int[] nums) {

        int n = nums.length;

        int l = 0;
        int r = n-1;

        while(l<=r){

            int m = (l+r)/2;

            //if left neighbor is greater
            if(m>0 && nums[m-1] > nums[m]){
                r = m-1;
            //right neighbor is greater
            }else if(m<n-1 && nums[m+1] > nums[m]){
                l = m+1;
            //current number must be more than both left and right
            }else{
                return m;
            }

        }

        return -1;

    }

}
```
