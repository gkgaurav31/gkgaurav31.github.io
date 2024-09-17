---
layout: post
title: Check for BST (geeksforgeeks - SDE Sheet)
date: 2024-09-17 15:31 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the root of a binary tree. Check whether it is a BST or not.
Note: We are considering that BSTs can not contain duplicate Nodes.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/check-for-bst/1?page=7)

## SOLUTION

```java
class Solution {

    // Define the range of valid values for the binary tree nodes
    private static final int MIN = 1;
    private static final int MAX = 100000;

    boolean isBST(Node root) {
        return isBSTHelper(root, MIN, MAX);
    }

    // 'minAllowed' and 'maxAllowed' define the valid range for the current node's value
    boolean isBSTHelper(Node root, int minAllowed, int maxAllowed) {

        // Base case: if the current node is null, it's a valid BST (empty tree is considered valid)
        if (root == null)
            return true;

        // Check if the current node's value violates the BST property
        // It must be within the allowed range [minAllowed, maxAllowed]
        if (root.data < minAllowed || root.data > maxAllowed) {
            return false;  // If out of bounds, it's not a valid BST
        }

        // Recursively check the left subtree, updating the max allowed value to root.data - 1
        // Recursively check the right subtree, updating the min allowed value to root.data + 1
        return isBSTHelper(root.left, minAllowed, root.data - 1) &&
               isBSTHelper(root.right, root.data + 1, maxAllowed);
    }
}
```
