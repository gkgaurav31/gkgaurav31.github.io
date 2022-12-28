---
layout: post
title: Count Good Nodes in Binary Tree
date: 2022-12-28 18:44 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

[Leetcode](https://leetcode.com/problems/count-good-nodes-in-binary-tree/description/)

### SOLUTION

```java
class Solution {
    
    public int goodNodes(TreeNode root) {
        if(root == null) return 0;
        return goodNodesHelper(root, root.val);
    }

    public int goodNodesHelper(TreeNode root, int max){

        if(root == null) return 0;

        if(root.val >= max){
            return 1 + goodNodesHelper(root.left, root.val) + goodNodesHelper(root.right, root.val);
        }else{
            return goodNodesHelper(root.left, max) + goodNodesHelper(root.right, max);
        }

    }

}
```
