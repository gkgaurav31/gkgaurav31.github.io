---
layout: post
title: Wiggle Sort
date: 2023-09-20 21:27 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an integer array nums, reorder it such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

You may assume the input array always has a valid answer.

[leetcode](https://leetcode.com/problems/wiggle-sort/)

## SOLUTION

### APPROACH 1

Original sorted array: [a, b, c, d, e]

First, we swap 'b' and 'c':

- a <= c (smaller or equal)
- b <= c (smaller or equal)
- c >= b (larger or equal)
- c >= d (larger or equal)

So, after the swap, it's still wiggly sorted: a <= c >= b <= d <= e
Next, we swap 'd' and 'e':

- b <= e (smaller or equal)
- e >= d (larger or equal)

So, after this swap, it's still wiggly sorted: a <= c >= b <= e >= d
In summary, swapping 'b' with 'c' and 'd' with 'e' didn't break the wiggly sorted order for 'a', 'b', and 'c'. That's why the array remains wiggly sorted.

```java
class Solution {

    public void wiggleSort(int[] nums) {

        int n = nums.length;

        Arrays.sort(nums); // Sort the input array in ascending order

        // Go to each odd index and swap it with its right element
        for(int i=1; i<=n-2; i+=2){
            swap(nums, i, i+1);
        }
    }

    public void swap(int[] nums, int i, int j){

        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;

    }

}
```

### APPROACH 2 (GREEDY)

This approach is actually similar to bubble sort.

```java
class Solution {

    public void wiggleSort(int[] nums) {

        int n = nums.length;

        // Iterate through the array from index 0 to n-2
        for(int i = 0; i <= n - 2; i++) {

            // If the current index is even (i%2 == 0)
            if (i % 2 == 0) {

                // Check if the current element is greater than the next element
                if (nums[i] > nums[i + 1]) {
                    // Swap the current element with the next element
                    swap(nums, i, i + 1);
                }

            } else { // If the current index is odd

                // Check if the current element is less than the next element
                if (nums[i] < nums[i + 1]) {
                    // Swap the current element with the next element
                    swap(nums, i, i + 1);
                }

            }

        }
    }

    public void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```
