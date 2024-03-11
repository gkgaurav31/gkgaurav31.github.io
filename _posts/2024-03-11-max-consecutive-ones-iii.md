---
layout: post
title: Max Consecutive Ones III
date: 2024-03-11 16:52 +0530
author: "Gaurav Kumar"
tags: "java leetcode sliding_window"
categories: "sliding_window"
---

## PROBLEM DESCRIPTION

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

## SOLUTION

```java
class Solution {

    public int longestOnes(int[] nums, int k) {

        int n = nums.length;

        // Initialize the maximum length of consecutive 1's to 0
        int max = 0;

        // sliding windows left and right pointer
        int l = 0;
        int r = 0;

        while(r < n){

            int current = nums[r];

            // If the current element is 1
            if(current == 1){

                // Calculate the current length of consecutive 1's
                int currentLength = r - l + 1;

                // Update the maximum length if needed
                max = Math.max(max, currentLength);

                // Move the right pointer to the next element
                r++;
            }

            // If the current element is 0
            else{
                // If we have used up all available flips (k <= 0)
                if(k <= 0){

                    // If the leftmost element is 0, we'll have to move the left pointer
                    if(nums[l] == 0){

                        // Increment k since we're moving past a 0
                        k++;
                    }
                    // Move the left pointer to the next element
                    l++;
                }
                // If we still have flips available (k > 0)
                else{
                    // Calculate the current length of consecutive 1's
                    int currentLength = r - l + 1;
                    // Update the maximum length if needed
                    max = Math.max(max, currentLength);

                    // Decrement k since we're flipping a 0 to 1
                    k--;
                    // Move the right pointer to the next element
                    r++;
                }
            }
        }

        // Return the maximum length of consecutive 1's
        return max;
    }
}
```
