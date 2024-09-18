---
layout: post
title: Symmetric Tree (geeksforgeeks - SDE Sheet)
date: 2024-09-18 21:40 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Tree. Check whether it is Symmetric or not, i.e. whether the binary tree is a Mirror image of itself or not.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/symmetric-tree/1?page=8)

## SOLUTION

```java
class GfG
{

    public static boolean isSymmetric(Node root)
    {
        return isSame(root, root);
    }

    public static boolean isSame(Node r1, Node r2){

        // If both nodes are null, they are symmetric (mirror images of each other).
        if(r1 == null && r2 == null)
            return true;

        // If one of the nodes is null and the other is not, they are not symmetric.
        if(r1 == null || r2 == null)
            return false;

        // If the data in the current nodes doesn't match, they are not symmetric.
        if(r1.data != r2.data)
            return false;

        // Recursively check if the left subtree of r1 is the mirror of the right subtree of r2
        // and the right subtree of r1 is the mirror of the left subtree of r2.
        return isSame(r1.left, r2.right) && isSame(r1.right, r2.left);
    }
}
```
