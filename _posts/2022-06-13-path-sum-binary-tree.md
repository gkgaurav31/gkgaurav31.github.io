---
layout: post
title: Path Sum - Binary Tree
date: 2022-06-13 02:25 +0530
tags: "java BST"
categories: "leetcode"
---

### PROBLEM DESCRIPTION: [Path Sum Binary Search Tree](https://leetcode.com/problems/path-sum/solution/)

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

### SOLUTION

```java
class Solution {
  public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null)
      return false;

    sum = sum - root.val;
    if ((root.left == null) && (root.right == null))
      return (sum == 0);
    return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
  }
}
```
