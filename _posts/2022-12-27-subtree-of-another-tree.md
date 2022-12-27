---
layout: post
title: Subtree of Another Tree
date: 2022-12-27 23:53 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

[Leetcode](https://leetcode.com/problems/subtree-of-another-tree/description/)

### SOLUTION

```java
class Solution {
    
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {

        if(root == null && subRoot == null) return false;
        if(root == null) return false; 

        boolean same = isSame(root, subRoot);
        return same || isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);

    }

    public boolean isSame(TreeNode p, TreeNode q){

        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        
        if(p.val != q.val) return false;

        return isSame(p.left, q.left) && isSame(p.right, q.right);
    }

}
```
