---
layout: post
title: Binary Tree Maximum Path Sum
date: 2022-10-04 23:27 +0530
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
The path sum of a path is the sum of the node's values in the path.
Given the root of a binary tree, return the maximum path sum of any non-empty path.  
[leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

### SOLUTION

Let's break this problem down to simpler problems.  

Let's say, we need to find the max path sum __STARTING__ from root. (it does not necessarily need to go till the leaf node)

We could solve in this way:

```java
int pathSum(TreeNode root){
    if(root == null) return 0;
    int l = pathSum(root.left);
    int r = pathSum(root.right);

    //We don't want to consider l or r if they are negative
    return root.val + Math.max(0, Math.max(l, r)); 
}
```

Now, let's say, we want to find the max sum path __CONTAINING__ the root node.  So, now we need to consider both left side path and right side path in the total sum.  

The logic would be:

```java
return root.data + Math.max(pathSum(LST), 0) + Math.max(pathSum(RST), 0);
```

Now, based on this, we can iterate through each node and calculate PathSum and update the maxSum whenever we get a higher value to get the max sum path (which does not need to include root).

```java
class Solution {
    
    int max = Integer.MIN_VALUE;
    
    public int maxPathSum(TreeNode root) {
        maxPathSumWithRoot(root);
        return max;
    }

    public int maxPathSumWithRoot(TreeNode root){

        if(root == null) return 0;

        int l = maxPathSumWithRoot(root.left);
        int r = maxPathSumWithRoot(root.right);
        
        //Here we are checking the max path sum which contains the current node
        //If it's greater, update the max 
        int m = root.val + Math.max(0,l) + Math.max(0, r);
        max = Math.max(m, max);

        //While returning, we need to be careful. We shouldn't be returning m because it includes the sum of both left side and right side.
        //For the next node, after returning, obviously we can only take one of them, not both.
        //So, we take the path which includes the root and has max sum.
        return root.val + Math.max(0, Math.max(l, r));
    }
}
```
