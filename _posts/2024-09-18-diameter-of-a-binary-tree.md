---
layout: post
title: Diameter of a Binary Tree (geeksforgeeks - SDE Sheet)
date: 2024-09-18 22:05 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The diameter of a tree (sometimes called the width) is the number of nodes on the longest path between two end nodes.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/diameter-of-binary-tree/1?page=8)

## SOLUTION

```java
class Solution {

    // Variable to store the maximum diameter found
    int max = 0;

    int diameter(Node root) {
        diameterHelper(root);
        return max;
    }

    int diameterHelper(Node root) {

        // Base case: if the node is null, return 0 (no height)
        if (root == null)
            return 0;

        // Recursively find the height of the left subtree
        int left = diameterHelper(root.left);

        // Recursively find the height of the right subtree
        int right = diameterHelper(root.right);

        // Calculate the combined length of the left and right path passing through the current node
        int combined = left + right + 1;

        // Update the maximum diameter encountered so far
        max = Math.max(max, Math.max(left + 1, Math.max(right + 1, combined)));

        // Return the height of the current subtree (which is the maximum height of either left or right plus 1)
        // We don't consider combined path here, since that cannot be used for the path in parent node
        return Math.max(left + 1, right + 1);
    }
}
```
