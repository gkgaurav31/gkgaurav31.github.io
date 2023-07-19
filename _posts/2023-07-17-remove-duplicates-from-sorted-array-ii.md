---
layout: post
title: Remove Duplicates from Sorted Array II
date: 2023-07-17 20:14 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.  

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.  

Return k after placing the final result in the first k slots of nums.  

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.  

[leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

## SOLUTION

We can keep two variables prev1 and prev2 to keep a track of previously seen values. If the current number is same as prev1 and prev2, skip it and update prev1 prev2. Otherwise, update it in the answer using a tracking pointer "p".

```java
class Solution {

    public int removeDuplicates(int[] nums) {

        int n = nums.length;

        //edge case
        if(n < 3) return n;

        //store the previous two values
        int prev1 = nums[0];
        int prev2 = nums[1];

        //pointer for next position to be updated
        int p = 2;

        //pointer for current element
        int c = 2;

        //while we reach end of array
        while(c<n){
            
            //if current element is not same as the earlier two elements:
            if(nums[c] != prev1 || nums[c] != prev2){

                //first we store the two previous elements so that we don't lose them
                prev1 = nums[c];
                prev2 = nums[c-1];

                //update the element to position p
                nums[p] = nums[c];

                //move p to store the next element
                p++;

            }

            //check next element
            c++;

        }

        //if p is at index 5, we must have filled items from [0, 4] => size 5
        return p;   

    }

}
```

## IMPROVING THE PREVIOUS CODE

In the code above, we were tracking the previous two values. However, it would be enough to just know the count of continuous same elements till position i. If the count is <=2, we can include the current element in the answer. If not, skip it. Anytime we get a different number, reset the count to 1.

```java
class Solution {

    public int removeDuplicates(int[] nums) {

        int n = nums.length;

        //edge case
        if(n < 3) return n;

        //track the count of the continuous elements
        int count = 1;

        //p: pointer to the index which will be updated next
        int p = 1;

        //c: pointer to check the next element in the array
        int c = 1;

        //while we reach end of array
        while(c<n){
            
            //if the current elemnt is same as previous, increase the count by 1
            if(nums[c] == nums[c-1]){
                count++;
            //else reset it to 1
            }else{
                count = 1;
            }

            //if the count is <=2, that should be included in the answer
            //so, update nums[p] and then move p to the next position
            if(count <= 2){
                nums[p] = nums[c];
                p++;
            }

            //go to next element in the array
            c++;

        }
        
        //if p is at index 5, we must have filled items from [0, 4] => size 5
        return p;

    }

}
```
