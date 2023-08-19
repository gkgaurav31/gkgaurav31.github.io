---
layout: post
title: Permutations
date: 2022-10-30 21:32 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

[leetcode](https://leetcode.com/problems/permutations/)

## Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> permute(int[] nums) {
        find(nums, 0);
        return ans;
    }

    public void save(int[] arr){
        List<Integer> list = new ArrayList<>();
        for(int i=0; i<arr.length; i++){
            list.add(arr[i]);
        }
        ans.add(list);
    }

    public void find(int[] arr, int i){

        if(i == arr.length-1){
            save(arr);
            return;
        }

        for(int j=i; j<arr.length; j++){
            swap(arr, i, j);
            find(arr, i+1);
            swap(arr, i, j);
        }

    }

    public void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}
```

## ANOTHER WAY TO CODE

```java
class Solution {

    public List<List<Integer>> permute(int[] nums) {
        return permuteHelper(nums, 0, new ArrayList<List<Integer>>());
    }

    // Helper function to swap elements at indices i and j in the array
    public void swap(int[] nums, int i, int j){
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

    // Helper function to generate all permutations using backtracking
    public List<List<Integer>> permuteHelper(int[] nums, int i, List<List<Integer>> all){

        // We are beyond the array now, so all positions have been covered
        // We can add the current "state" of the nums array as one of the possible permutations
        if(i == nums.length){

            // Create a new list to store the current permutation
            List<Integer> permutation = new ArrayList<>();
            for (int num : nums) {
                permutation.add(num);
            }

            // Add the current permutation to the list of all permutations
            all.add(permutation);

            // Return the updated list of permutations
            return all;
        }

        // Iterate through the array, swapping elements at index i with elements at index j
        for(int j=i; j<nums.length; j++){

            // Swap elements at indices i and j to create a new permutation
            swap(nums, i, j);

            // Recursively generate permutations for the next element
            permuteHelper(nums, i+1, all);

            // Swap back elements at indices i and j to backtrack and try other possibilities
            swap(nums, i, j);

        }

        // Return the list of permutations
        return all;
    }
}
```
