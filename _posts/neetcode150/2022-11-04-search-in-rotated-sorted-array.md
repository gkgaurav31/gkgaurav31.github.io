---
layout: post
title: Search in Rotated Sorted Array
date: 2022-11-04 22:39 +0530
author: "Gaurav Kumar"
tags: "java binary_search neetcode150"
categories: "neetcode150"
---

## Problem Description

There is an integer array nums sorted in ascending order (with distinct values).  

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].  

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.  

You must write an algorithm with O(log n) runtime complexity.  

[leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

### Solution

One way to solve is by doing the following:

- Find the pivot element by binary search. If the arr[mid] is less than arr[0], we know we should look towards left, else we look towards right. We can return when arr[mid] < arr[mid-1] because the array is sorted in ascending order.

- Once this is done, we have two subarrays which are both sorted. We can apply binary search on each of them and look for the target value.

```java
class Solution {
    public int search(int[] nums, int target) {

        int p = pivotIndex(nums);

        System.out.print("Pivot:" + p);

        int x = search(nums, 0, p-1, target);
        if(x != -1) return x;

        int y = search(nums, p, nums.length-1, target);
        if(y != -1) return y;

        return -1;

    }

    public int search(int[] nums, int l, int r, int target){
        while(l<=r){
            int m=(l+r)/2;
            if(nums[m] == target) return m;
            else if(nums[m] < target){
                l = m+1;
            }else{
                r= m-1;
            }

        }
        return -1;
    }

    public int pivotIndex(int[] nums){

        int l=0;
        int r=nums.length-1;

        while(l<=r){
            
            int m = (l+r)/2;

            if(m>=1 && nums[m] < nums[m-1]){
                return m;
            }

            if(nums[m] < nums[0]){
                r=m-1;
            }else{
                l=m+1;
            }

        }

        return 0;

    }
}
```
