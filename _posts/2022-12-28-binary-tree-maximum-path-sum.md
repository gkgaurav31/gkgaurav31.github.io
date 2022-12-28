---
layout: post
title: Binary Tree Maximum Path Sum
date: 2022-12-28 23:05 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

[Leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

### SOLUTION

```java
class Solution {
    
    public int maxPathSum(TreeNode root) {
        maxPathHelper(root);
        return max;
    }

    int max = Integer.MIN_VALUE;

    public int maxPathHelper(TreeNode root){

        if(root == null) return 0;

        int l = maxPathHelper(root.left);
        int r = maxPathHelper(root.right);

        int total = l + r + root.val;
        
        //update max based on:
        //current max value OR current node value OR current node + LST path sum OR current node + RST path sum OR current node + LST + RST
        max =  Math.max(max, Math.max(root.val, Math.max(root.val+l, Math.max(root.val+r, total))));

        //we return only max between current value OR current + LSR OR current + RST
        //this is done, because for the next node, we cannot take both left and right in the path
        return Math.max(root.val, Math.max(root.val+l, root.val+r));

    }

}
```
