---
layout: post
title: Minimum Absolute Difference in BST
date: 2023-08-13 17:35 +0530
author: "Gaurav Kumar"
tags: "java BST leetcode leetcode150"
categories: "BST"
---
## PROBLEM DESCRIPTION

Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

[leetcode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## SOLUTION

We should be aware that the inorder traversal of a BST will be sorted. The minimum absolute value will be a difference in one of the pair of adjacent values in the sorted list of values.

```java
class Solution {

    public int getMinimumDifference(TreeNode root) {

        // Perform an in-order traversal to get the values of the nodes in sorted order
        List<Integer> inorder = inorder(root, new ArrayList<>());

        // init to max as we are looking for min value
        int minAbs = Integer.MAX_VALUE; 

        // Calculate the minimum absolute difference between adjacent values (it is given that min number of nodes is 2)
        for (int i = 1; i < inorder.size(); i++) {
            minAbs = Math.min(minAbs, Math.abs(inorder.get(i) - inorder.get(i - 1)));
        }

        return minAbs; 
    }

    // Recursive function for in-order traversal
    public List<Integer> inorder(TreeNode root, List<Integer> list) {
        if (root == null) return list; 

        inorder(root.left, list); 
        list.add(root.val); 
        inorder(root.right, list); 

        return list; 
    }
}
```
