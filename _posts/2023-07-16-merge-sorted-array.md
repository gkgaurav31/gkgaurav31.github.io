---
layout: post
title: Merge Sorted Array
date: 2023-07-16 21:35 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

[leetcode](https://leetcode.com/problems/merge-sorted-array)

## SOLUTION

Using using extra space, the problem can be solved easily. To solve the problem in-place without using any extra space, we need to use a different strategy. The main observation is that the extra space in the nums1 array which has all 0s can be used to store the elements. If we would have started the comparison of elements from the start of the arrays, we would lose data as we would need to override them.  

Instead of doing this, we can start out loop from the end of the arrays:

- p1 points to the last valid element in nums1.
- p2 points to the last element in nums2.
- p points to the last available position in the merged array (nums1).

Now, if p2 points to a larger value, put that on the position indicated by "p" and reduce both the pointers. Similarly, if p1 points to a larger value, put that value on the position indicated by "p" and reduce both the pointers.  

At the end, it may be possible that p1 is -1 but p2 is >=0 if it has more elements which were smaller. So, we will just add a while loop to include the remaining elements.

```java
class Solution {

    public void merge(int[] nums1, int m, int[] nums2, int n) {

        int p1 = m-1;
        int p2 = n-1;

        int p = n+m-1;

        while(p1 >= 0 && p2 >= 0){

            if(nums1[p1] >= nums2[p2]){
                nums1[p] = nums1[p1];
                p1--;
            }else{
                nums1[p] = nums2[p2];
                p2--;
            }

            p--;

        }   

        while(p2 >= 0){
            nums1[p2] = nums2[p2];
            p2--;
        }

    }
}```
