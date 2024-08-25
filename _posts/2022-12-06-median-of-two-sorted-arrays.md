---
layout: post
title: Median of Two Sorted Arrays
date: 2022-12-06 23:08 +0530
author: "Gaurav Kumar"
tags: "java binary_search search neetcode150 important"
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

### SIMILAR CODE

```java
class Solution {

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        int n = nums1.length;
        int m = nums2.length;

        // make the first array shorter
        // we will use binary search on nums1
        if(n > m)
            return findMedianSortedArrays(nums2, nums1);

        // total number of elements
        int total = n+m;

        // we need half of total to figure out the median
        int half = total/2;

        // init: for binary search
        int l = 0; // take 0 elements from nums1
        int r = n; // take n elements from nums1

        while(l <= r){

            // i -> partition position for first array
            int i = (l + r + 1)/2;

            // j -> if we take x elements from first array, we will take (half-x) from second array to figure out the median
            int j = half - i;

            // init: left of partition index as MIN
            // init: right of partition index as MAX
            int l1 = Integer.MIN_VALUE, l2 = Integer.MIN_VALUE;
            int r1 = Integer.MAX_VALUE, r2 = Integer.MAX_VALUE;

            if(i-1 >= 0)
                l1 = nums1[i-1]; // left of partition position in arr1
            if(i < n)
                r1 = nums1[i];  // at the partition position in arr1

            if(j-1 >=0)
                l2 = nums2[j-1]; // left of partition position in arr2

            if(j < m)
                r2 = nums2[j]; // at the partition position in arr2

            if(l1 <= r2 && l2 <= r1){ // check if this can form the first half correctly

                // if even length
                if(total%2 == 0){

                    return (Math.max(l1, l2) + Math.min(r1, r2))/2.0;

                // for odd length
                }else{

                    return Math.min(r1, r2)/1.0;

                }

            // if arr1 partition has greater element than arr2 partition, we need to reduce arr1 partition
            }else if(l1 > r2){
                r = i - 1;
            // otherwise, we need to add more elements to arr1 partition
            }else
                l = i + 1;

        }

        return -1.0;

    }

}
```
