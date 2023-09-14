---
layout: post
title: Median of Two Sorted Arrays
date: 2022-12-06 23:08 +0530
author: "Gaurav Kumar"
tags: "java binary_search search neetcode150"
categories: "neetcode150"
---

## Problem Description

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.  
The overall run time complexity should be O(log (m+n)).

[leetcode](https://leetcode.com/problems/median-of-two-sorted-arrays/)

### Solution

[NeetCode Youtube](https://www.youtube.com/watch?v=q6IEA26hvXc)

```java
class Solution {

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        //handle empty arrays
        if(nums1.length == 0 && nums2.length ==0)
            return 0;

        if(nums1.length == 0)
            return nums2.length%2 == 0 ? (nums2[nums2.length/2] + nums2[(nums2.length-1)/2] )/2.0 : nums2[nums2.length/2];

        if(nums2.length == 0)
            return nums1.length%2 == 0 ? (nums1[nums1.length/2] + nums1[(nums1.length-1)/2] )/2.0 : nums1[nums1.length/2];


        //ensure nums1 is of smaller length
        //we will apply Binary Search on smaller array for better time complexity
        if(nums1.length > nums2.length){
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }

        //init
        int n = nums1.length;
        int m = nums2.length;
        int total = n+m;
        int half = total/2; //size of partition needed

        //binary search on nums1
        int l=0;
        int r=nums1.length;

        while(true){

            //IMPORTANT: https://www.geeksforgeeks.org/python-operators/
            //int i = (l+r)/2 will not work. We need to floor value which is equivalent of // in Python
            int i = (int)Math.floor((r+l)/2.0);

            //half - (number of elements till i) - 1 because 0 indexed
            //=> half - (i+1) - 1 = half - i - 2;
            int j = half - i - 2;

            //get the elements which need to be compared
            int nums1Left = (i>=0?nums1[i]:Integer.MIN_VALUE);
            int nums1Right = (i+1<nums1.length?nums1[i+1]:Integer.MAX_VALUE);

            int nums2Left = (j>=0?nums2[j]:Integer.MIN_VALUE);
            int nums2Right = (j+1<nums2.length?nums2[j+1]:Integer.MAX_VALUE);

            //valid partition
            if(nums1Left <= nums2Right && nums2Left <= nums1Right){
                //odd
                if(total%2 == 1){
                    return Math.min(nums1Right, nums2Right);
                }//even
                else{
                    return (double)(Math.max(nums1Left, nums2Left) + Math.min(nums1Right, nums2Right)) / 2;
                }
            //need to take lesser elements in nums1 partition
            }else if(nums1Left > nums2Right){
                r = i-1;
            //else need more elements in nums1 partition
            }else{
                l = i+1;
            }

        }

    }
}
```

### ANOTHER WAY TO CODE

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        if (nums1.length > nums2.length) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }

        int n = nums1.length;
        int m = nums2.length;

        int total = n + m;
        int half = total / 2;

        int l = 0;
        int r = n;

        while (l <= r) {

            int i = (l + r) / 2;
            int j = half - i;

            int aLeft = (i > 0) ? nums1[i - 1] : Integer.MIN_VALUE;
            int aRight = (i < n) ? nums1[i] : Integer.MAX_VALUE;
            int bLeft = (j > 0) ? nums2[j - 1] : Integer.MIN_VALUE;
            int bRight = (j < m) ? nums2[j] : Integer.MAX_VALUE;

            if (aLeft <= bRight && bLeft <= aRight) {

                if (total % 2 == 1) {
                    return Math.min(aRight, bRight);
                } else {
                    return (Math.max(aLeft, bLeft) + Math.min(aRight, bRight)) / 2.0;
                }

            } else if (aLeft > bRight) {
                r = i - 1;
            } else {
                l = i + 1;
            }

        }

        return 0;
    }
}
```
