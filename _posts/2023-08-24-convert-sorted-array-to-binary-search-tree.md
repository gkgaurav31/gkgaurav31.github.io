---
layout: post
title: Convert Sorted Array to Binary Search Tree
date: 2023-08-24 19:50 +0530
author: "Gaurav Kumar"
tags: "java divide_and_conquer binarytree leetcode leetcode150"
categories: "binarytree"
---

## PROBLEM DESCRIPTION

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.  
[leetcode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## SOLUTION

We can solve this recursively:

- First get the middle element from range [start, end]. Let's say it's idx.
- Create a node using the value present at idx index.
- Now recursively solve the sub-problem for left and right sub-trees for this node.
  - The left sub-tree will be recur call to func(start, idx-1)
  - The right sub-tree will be recur call to func(idx+1, end)

```java
class Solution {

    public TreeNode sortedArrayToBST(int[] nums) {
        return builder(nums, 0, nums.length-1);
    }

    public TreeNode builder(int[] nums, int i, int j) {

        // Base case: When the left index is greater than the right index, return null.
        if (i > j) {
            return null;
        }

        // Calculate the index of the middle element.
        int idx = (i + j) / 2;

        // Create a new tree node with the middle element as the root value.
        TreeNode node = new TreeNode(nums[idx]);

        // Recursively build the left subtree using elements from the left half of the array.
        node.left = builder(nums, i, idx - 1);

        // Recursively build the right subtree using elements from the right half of the array.
        node.right = builder(nums, idx + 1, j);

        return node;
    }

}
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```
