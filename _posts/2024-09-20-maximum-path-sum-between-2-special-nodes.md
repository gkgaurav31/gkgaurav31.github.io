---
layout: post
title: Maximum Path Sum between 2 Special Nodes (geeksforgeeks - SDE Sheet)
date: 2024-09-20 22:05 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree in which each node element contains a number. Find the maximum possible path sum from one special node to another special node.

Note: Here special node is a node which is connected to exactly one different node.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-path-sum/1?page=8)

## SOLUTION

Leaf nodes are considered special nodes. If the root node does not have a left or right subtree, it will be connected to only a single node and thus treated as a special node. When we reach a leaf node during iteration, we can directly return its value.

For internal nodes, if the current node has both left and right children, we recursively find the maximum path sum for the left and right subtrees. The potential maximum path sum at this node will be the sum of the maximum values from both subtrees plus the current node’s value `(max(left_max, right_max) + node.val)`. We update the global maximum if this new sum is greater.

If either the left or right child of the current node is null, we do not update the global maximum. This node will be connected to either its left or right child, as well as its parent. An edge case exists for the root node: if it has either its left or right child as null, it should be considered separately since it will become a special node.

```java
class Solution
{

    int max = Integer.MIN_VALUE;

    int maxPathSum(Node root)
    {

        int h = maxPathSumHelper(root);

        // IMPORTANT:
        // in our recursive method maxPathSumHelper, the max gets updated
        // only when both left and right sub trees are present
        // so, if the root itself does not have either left or right subtree,
        // max will never get updated
        // we need to handle this case by including root as well
        // in that case, the max will be the max between left path or right path
        // we would have got this result already from maxPathSumHelper call
        if(root.left == null || root.right == null){
            max = Math.max(max, h);
        }

        return max;

    }

    int maxPathSumHelper(Node root){

        if(root == null)
            return 0;

        // leaf
        if(root.left == null && root.right == null){
            return root.data;
        }


        if(root.left != null && root.right != null){

            int ls = maxPathSumHelper(root.left);
            int rs = maxPathSumHelper(root.right);

            max = Math.max(max, ls + rs + root.data);

            return Math.max(ls + root.data, rs + root.data);

        }

        if(root.left == null){
            return maxPathSumHelper(root.right) + root.data;
        }else{
            return maxPathSumHelper(root.left) + root.data;
        }

    }

}
```
