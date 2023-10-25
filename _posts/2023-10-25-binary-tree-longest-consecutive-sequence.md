---
layout: post
title: Binary Tree Longest Consecutive Sequence
date: 2023-10-25 21:23 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcodealgo100 important"
categories: "binarytree"
---

## PROBLEM DESCRIPTION

Given the root of a binary tree, return the length of the longest consecutive sequence path.
A consecutive sequence path is a path where the values increase by one along the path.
Note that the path can start at any node in the tree, and you cannot go from a node to its parent in the path.

[leetcode](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

## SOLUTION

```java
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
class Solution {

    int maxLength;  // Initialize a variable to store the maximum length of consecutive sequence path.

    public int longestConsecutive(TreeNode root) {
        dfs(root);  // Start the DFS traversal from the root node.
        return maxLength;  // Return the calculated maximum length.
    }

    public void dfs(TreeNode root) {
        if (root == null) return;  // If the current node is null, return (base case for recursion).

        longestConsecutive(root.left);  // Recursively traverse the left subtree.
        helper(root);  // Call the helper function to calculate the consecutive sequence length.
        longestConsecutive(root.right);  // Recursively traverse the right subtree.
    }

    public int helper(TreeNode root) {
        if (root == null) return 0;  // If the current node is null, return 0 (base case for recursion).

        int left = 1;  // Initialize a variable to store the length of consecutive sequence in the left subtree.
        int right = 1;  // Initialize a variable to store the length of consecutive sequence in the right subtree.

        // Check if there is a consecutive sequence in the left subtree.
        if (root.left != null && root.left.val == root.val + 1) {
            left = 1 + helper(root.left);  // Update the left length and recursively explore the left subtree.
        }

        // Check if there is a consecutive sequence in the right subtree.
        if (root.right != null && root.right.val == root.val + 1) {
            right = 1 + helper(root.right);  // Update the right length and recursively explore the right subtree.
        }

        int currentLength = Math.max(left, right);  // Calculate the current consecutive sequence length.
        maxLength = Math.max(maxLength, currentLength);  // Update the maximum length if necessary.

        return currentLength;  // Return the current consecutive sequence length.
    }
}
```
