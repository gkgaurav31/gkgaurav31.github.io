---
layout: post
title: Lowest Common Ancestor in a BST (geeksforgeeks - SDE Sheet)
date: 2024-09-18 21:51 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Search Tree (with all values unique) and two node values n1 and n2 (n1!=n2). Find the Lowest Common Ancestors of the two nodes in the BST.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/lowest-common-ancestor-in-a-bst/1?page=8)

## SOLUTION

```java
class BST
{
    // LCA: lowest node in the tree that has both n1 and n2 as descendants.
    Node LCA(Node root, int n1, int n2)
    {
        // If the root is null, there is no tree, so return null.
        if(root == null)
            return null;

        // If both n1 and n2 are smaller than the root's data, the LCA must be in the left subtree.
        if(n1 < root.data && n2 < root.data)
            return LCA(root.left, n1, n2);

        // If both n1 and n2 are greater than the root's data, the LCA must be in the right subtree.
        else if(n1 > root.data && n2 > root.data)
            return LCA(root.right, n1, n2);

        // If one of n1 or n2 is on one side of the root and the other is on the other side,
        // or if one of them matches the root itself, the current node is the LCA.
        else
            return root;
    }
}
```
