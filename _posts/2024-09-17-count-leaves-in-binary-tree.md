---
layout: post
title: Count Leaves in Binary Tree (geeksforgeeks - SDE Sheet)
date: 2024-09-17 15:04 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Tree of size N, You have to count leaves in it.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/count-leaves-in-binary-tree/1?page=7)

## SOLUTION

This can be solved by recursively traversing the binary tree and counting its leaf nodes. The countLeaves() function checks if the current node is null; if it is, the function returns 0, meaning there are no leaves at this point. If the node is a leaf (i.e., it has no left or right children), the function returns 1, counting this node as a leaf. Otherwise, the function recursively calls itself on both the left and right child nodes, summing up the total number of leaves found in both subtrees.

```java
class Tree
{
    int countLeaves(Node node)
    {

         if(node == null)
            return 0;

         if(node.left == null && node.right == null)
            return 1;

         return countLeaves(node.left) + countLeaves(node.right);

    }
}
```
