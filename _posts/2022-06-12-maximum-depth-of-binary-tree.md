---
layout: post
title: Maximum Depth of Binary Tree
date: 2022-06-12 23:31 +0530
tags: "java binarytree"
categories: "leetcode"
---

## Problem Description:  [Maximum Depth of Binary Tree](https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/535/)

### Solution

The bottom-up approach will look like this:

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
    
    public int maxDepth(TreeNode root) {
        
        if(root == null) return 0;
        
        int left_depth = maxDepth(root.left);
        int right_depth = maxDepth(root.right);
        
        return Math.max(left_depth, right_depth) + 1;
        
    }
    
}
```
