---
layout: post
title: Validate Binary Search Tree
date: 2024-03-02 18:48 +0530
author: "Gaurav Kumar"
tags: "java leetcode binarytree neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

- The left
- subtree
- of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

[leetcode](https://leetcode.com/problems/validate-binary-search-tree/description/)

### SOLUTION

For each node, we will try to figure out the range of values it can have. If it does not contain within that range, we can return false. Otherwise, we check the same for its child nodes.

The first node can have any value. So, we start with `[LONG_MIN, LONG_MAX]`. The left nodes should have values less than it and right nodes should have values more than that. We can also say, the left node value should be in the range `[LONG_MIN, currentVal)` and the right node value should be in the range `(currentVal, LONG_MAX]`. We use this logic recursively for each node.

The root value can have a maximum value of INT_MIN or INT_MAX as well, that's why we need to use a higher range to start with.

```java
class Solution {

    public boolean isValidBST(TreeNode root) {
        return validHelper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean validHelper(TreeNode root, long min, long max){

        if(root == null)
            return true;

        if(root.val <= min || root.val >= max){
            return false;
        }

        return validHelper(root.left, min, root.val) && validHelper(root.right, root.val, max);

    }

```
