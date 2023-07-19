---
layout: post
title: Rotate an Array k times
author: "Gaurav Kumar"
tags: "java leetcode leetcode150 arrays"
categories: "arrays"

---

## Problem Description  

Given an Array, rotate it k times

### Example

__Array:__
1 2 3 4 5 6 7 8 9  
__k: 3__  
__Output:__
7 8 9 1 2 3 4 5 6  

[leetcode](https://leetcode.com/problems/rotate-array/)

### Solution

Array: 1 2 3 4 5 6 7 8 9  
k: 3

In the output array, we can see that the last k elements will appear first, and then the rest of the elements will appear.
So, step 1: reverse the array to get [9 8 7 6 5 4 3 2 1]
The, use the "reverse an array in the given range code" to reverse the first 3 elements. Do the same for remaining elements. This will give us the final answer.

:exclamation: | Since k can be greater than the array length, we will need to use: __k = k%arr.length__

```java
class Solution {

    public void rotate(int[] nums, int k) {

        int n = nums.length;

        k = k%n;

        reverseRange(nums, 0, n-1);
        reverseRange(nums, 0, k-1);
        reverseRange(nums, k, n-1);

    }

    public void reverseRange(int[] nums, int start, int end){

        int i=start;
        int j=end;

        while(i<j){
            swap(nums, i, j);
            i++;
            j--;
        }
    }

    public void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
