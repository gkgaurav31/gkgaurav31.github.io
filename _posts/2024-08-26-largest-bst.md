---
layout: post
title: Largest BST
date: 2024-08-26 17:21 +0530
author: "Gaurav Kumar"
tags: "java BST binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree. Find the size of its largest subtree which is a Binary Search Tree.
Note: Here Size equals the number of nodes in the subtree.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/largest-bst/1?page=3)

## SOLUTION

```java
class Solution{

    static int MIN = 0;
    static int MAX = 1000001;

    // return the size of the largest sub-tree which is also a BST
    static int largestBst(Node root)
    {
        if(root == null)
            return 0;

        // if root is already a valid BST, then we don't need to check its sub trees since they cannot has larger size
        if(isBST(root, MIN, MAX))
            return size(root);

        return Math.max(largestBst(root.left), largestBst(root.right));
    }

    // get the number of nodes in the current binary tree
    static public int size(Node root){

        if(root == null)
            return 0;

        int lst = size(root.left);
        int rst = size(root.right);

        return 1 + lst + rst;

    }

    // check if the current binary tree is a valid BST
    static public boolean isBST(Node root, int minVal, int maxVal){

        if(root == null)
            return true;

        // as per the example, duplicates should go to left tree
        if(root.data < minVal || root.data >= maxVal)
            return false;

        // since duplicates are allowed and they need to go to left tree
        // we set the maxValue on left to be the same as current root value
        boolean lst = isBST(root.left, minVal, root.data);
        boolean rst = isBST(root.right, root.data+1, maxVal);

        return lst && rst;
    }

}
```
