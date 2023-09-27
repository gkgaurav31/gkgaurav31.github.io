---
layout: post
title: Max Consecutive Ones II
date: 2023-09-27 20:01 +0530
author: "Gaurav Kumar"
tags: "java sliding_window leetcode leetcodealgo100"
categories: "sliding_window"
---

## PROBLEM DESCRIPTION

Given a binary array nums, return the maximum number of consecutive 1's in the array if you can flip at most one 0.

[leetcode](https://leetcode.com/problems/max-consecutive-ones-ii/)

## SOLUTION

```java
class Solution {

    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;

        int i = 0;            // left pointer i to the beginning of the window.
        int j = 0;            // right pointer j to the end of the window.

        int flipped = 0;      // Initialize a variable to keep track of the number of flips made.

        int maxLength = 0;

        // While there are no elements left in nums array (tracked using j pointer which is the end of the window)
        while (j < n) {

            // If the current element at j is 1, it's a consecutive one.
            if (nums[j] == 1) {

                // Increment the current consecutive ones length and move the right pointer.
                maxLength = Math.max(maxLength, j - i + 1);
                j++;

            } else {  // If the current element at j is 0, we need to consider a flip.

                // If we haven't used our flip yet (flipped is 0):
                if (flipped == 0) {

                    // Increment the current consecutive ones length as we can flip the current 0 to 1.
                    maxLength = Math.max(maxLength, j - i + 1);

                    flipped = 1;  // Set flipped to 1 to indicate that we've used our flip.
                    j++;         // Move the right pointer to the next element.

                } else {  // If we've already used our flip:

                    // reduce the window from left side until we get a 0
                    // when we are at that position, we know we must have flipped it already (because "flipped" is 1 at this point)
                    // at that point, we can reset flip and start from the next position after that 0
                    // note that, we have not incremented j at this point. So, flipped is still 0. In the next iteration, nums[j] will be 0 and flipped=0, in which case we will cover flipping this position as well.
                    // alternatively, we can increment j here and keep flipped as 1. That will also work.
                    while (nums[i] == 1) {
                        i++;
                    }
                    flipped = 0;  // Reset flipped to 0, as we are going to flip a 0.
                    i++;         // Move the left pointer to the next element to consider a flip.

                }

            }

        }

        return maxLength;  // Return the maximum consecutive ones length with at most one flip.

    }

}
```
