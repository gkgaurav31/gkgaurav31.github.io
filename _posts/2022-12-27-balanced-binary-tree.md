---
layout: post
title: Balanced Binary Tree
date: 2022-12-27 23:47 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Given a binary tree, determine if it is height-balanced. A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

[Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/)

### SOLUTION

```java
class Solution {
    public boolean isBalanced(TreeNode root) {

        if(root == null) return true;
        
        int l = depth(root.left);
        int r = depth(root.right);

        int diff = Math.abs(l-r);

        return diff<=1 && isBalanced(root.left) && isBalanced(root.right);

    }

    public int depth(TreeNode root){

        if(root == null) return 0;

        int l = depth(root.left);
        int r = depth(root.right);

        return 1 + Math.max(l,r);

    }

}
```
