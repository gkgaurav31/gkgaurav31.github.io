---
layout: post
title: Binary Tree Maximum Path Sum
date: 2022-10-04 23:27 +0530
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
The path sum of a path is the sum of the node's values in the path.
Given the root of a binary tree, return the maximum path sum of any non-empty path.  
[leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

### SOLUTION

```java
class Solution {
    
    int max = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        maxPathSumWithRoot(root);
        return max;
    }

    public int maxPathSumWithRoot(TreeNode root){
        if(root == null) return 0;
        int l = maxPathSumWithRoot(root.left);
        int r = maxPathSumWithRoot(root.right);
        int m = root.val + Math.max(0,l) + Math.max(0, r);
        max = Math.max(m, max);
        return root.val + Math.max(0, Math.max(l, r));
    }
}
```
