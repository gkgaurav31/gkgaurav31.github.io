---
layout: post
title: Height Balanced Binary Tree
date: 2022-06-27 01:58 +0530
author: "Gaurav Kumar"
tags: "java binarytree"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

A binary tree is balanced if for each node in the tree, the difference between the height of its LST and RST is at most 1.

### SOLUTION

```java
  public int heightBalancedBinaryTreeX(BinaryTree tree) {

    if(tree == null) return 0;
    
    int left = heightBalancedBinaryTreeX(tree.left);
    int right = heightBalancedBinaryTreeX(tree.right);

    return 1 + Math.max(left, right);
    
  }

  public boolean heightBalancedBinaryTree(BinaryTree tree) {

    if(tree == null) return true;

    int left = heightBalancedBinaryTreeX(tree.left);
    int right = heightBalancedBinaryTreeX(tree.right);
    
    return Math.abs(left-right) <= 1  && heightBalancedBinaryTree(tree.left) && heightBalancedBinaryTree(tree.right);
    
  }
```
