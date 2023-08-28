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

## Solution

### APPROACH 1

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

### APPROACH 2 (SINGLE BINARY SEARCH)

```java
class Solution {

    public int search(int[] nums, int target) {

        int n = nums.length;

        int l=0;
        int r=n-1;

        while(l<=r){

            int m = (l+r)/2;

            //if nums[m] matches the target, then return index m
            if(nums[m] == target){
                return m;
            }

            //we will try to figure out if we are at the first half of the sorted array or second half
            //we can do this by comparing the current element with the left most element

            //left part, since current element is more than the left most element
            if(nums[m] >= nums[l]){

                //if we are on the left part and the number we are looking for is greater, go to right
                if(target > nums[m])
                    l = m+1;

                //IMPORTANT:
                //if we are on the left part but the target is lesser value we have two choices actually:
                //1. go to the left of first part of the array
                //2. go to right (which will also include the right part of the array)

                //How can we decide between the above two options?
                //[4,5,6,7,0,1,2]
                //let's say mid index is 3
                //if the target is lesser than the left most element nums[l], then we must go right
                else if(target < nums[l]){
                    l = m+1;
                //otherwise, we can go left
                }else{
                    r = m-1;
                }

            //otherwise, right part
            }else{

                //[4,5,6,7,0,1,2]
                //let's say mid index is 5
                //if we are on the right part and target is lesser than current element, go left
                if(target < nums[m]){
                    r = m-1;

                //if we are on the right part and target is more than the current element, we have two choices:
                //1. go to right part of second array
                //2. go to left, which will include the left part of the array as well

                //How can we decide between the above two options?
                //if the target is more than the largest number which is at nums[r], then we must go left
                }else if(target > nums[r]){
                    r = m-1;
                //otherwise, we can go right
                }else{
                    l = m+1;
                }

            }

        }

        return -1;

    }

}
```
