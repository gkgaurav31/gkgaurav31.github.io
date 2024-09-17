---
layout: post
title: Balanced Tree Check (geeksforgeeks - SDE Sheet)
date: 2024-09-17 15:56 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree, find if it is height balanced or not. A tree is height balanced if difference between heights of left and right subtrees is not more than one for all nodes of tree.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/check-for-balanced-tree/1?page=7)

## SOLUTION

### APPROACH 1 - O(N^2) - ACCEPTED

```java
class Tree
{
    boolean isBalanced(Node root)
    {
        // An empty tree is considered balanced.
        if(root == null)
            return true;

        // Get the height of the left and right subtrees.
        int left = height(root.left);
        int right = height(root.right);

        // If the difference in height between left and right subtrees is more than 1, the tree is not balanced.
        if(Math.abs(left - right) > 1)
            return false;

        // Recursively check if the left and right subtrees are balanced.
        return isBalanced(root.left) && isBalanced(root.right);
    }

    // Function to calculate the height of a binary tree.
    int height(Node root)
    {
        // An empty tree has a height of 0.
        if(root == null)
            return 0;

        // Calculate height as 1 plus the maximum height of left and right subtrees.
        return 1 + Math.max(height(root.left), height(root.right));
    }
}
```

### APPROACH 2 - OPTIMIZED

[Good Explanation - takeUForward](https://www.youtube.com/watch?v=Yt50Jfbd8Po)

```java
class Tree
{
    boolean isBalanced(Node root)
    {
        return heightHelper(root) == -1 ? false : true;
    }

    int heightHelper(Node root)
    {
        // An empty tree has a height of 0 and is considered balanced.
        if(root == null)
            return 0;

        // Recursively get the height of the left and right subtrees.
        int lst = heightHelper(root.left);
        int rst = heightHelper(root.right);

        // If either subtree is not balanced (returns -1), the current tree is also not balanced.
        if(lst == -1 || rst == -1)
            return -1;

        // If the difference in height between the left and right subtrees is more than 1, the tree is not balanced.
        if(Math.abs(lst - rst) > 1)
            return -1;

        // Return the height of the current node as 1 plus the maximum height of its subtrees.
        return 1 + Math.max(lst, rst);
    }
}
```
