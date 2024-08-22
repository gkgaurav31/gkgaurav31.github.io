---
layout: post
title: Merge Sorted Array
date: 2024-08-21 22:55 +0530
author: "Gaurav Kumar"
tags: "java sorting leetcode microsoft important"
categories: "microsoft30days"
---

## Problem Description

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

[leetcode](https://leetcode.com/problems/merge-sorted-array/?envType=company&envId=microsoft&favoriteSlug=microsoft-thirty-days)

## Solution

This problem is actually not so simple. The most important thing to observe here is that it would help if we start from right side of the two arrays and keep moving the larger elements on the right side of the extra space in the first array.

- pointer1 for arr1 will start from m-1
- pointer2 for arr2 will start from n-1;
- pointer3: we will keep adding elements from right to left in nums1 array from n+m-1

We can get into three conditions:

- both i and j are >= 0:
  so compare the elements in both arr1 and arr2. Move the larger element on the right side using pointer3. Decrease pointer3 by 1 position for the next larger element
- only i is >= 0:
  there are no elements in j left, so element in arr1 becomes the next one at pointer3 position
- only j is >=0:
  there are no elements in i left, so element in arr2 becomes the next one at pointer3 position

```java
class Solution {

    public void merge(int[] nums1, int m, int[] nums2, int n) {

        int i = m-1;
        int j = n-1;

        int idx = n+m-1;

        while(idx >=0){

            if(i >= 0 && j >= 0){
                nums1[idx] = nums1[i] > nums2[j] ? nums1[i--] : nums2[j--];
            }else if(i >= 0){
                nums1[idx] = nums1[i--];
            }else{
                nums1[idx] = nums2[j--];
            }

            idx--;

        }


    }

}
```
