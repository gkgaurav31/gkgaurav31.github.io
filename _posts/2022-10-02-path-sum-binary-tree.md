---
layout: post
title: Path Sum - Binary Tree
date: 2022-10-02 15:48 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.
[leetcode](https://leetcode.com/problems/path-sum/)

### SOLUTION

```java
class Solution {
    
    public boolean hasPathSum(TreeNode root, int targetSum) {
        
        if(root == null) return false;
        
        targetSum = targetSum - root.val;
        
        if(targetSum == 0 && (root.left == null && root.right == null)) return true;
        
        return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
        
    }
}
```
