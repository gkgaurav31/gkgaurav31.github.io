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
        //root.val will be the current maximum for root 
        return goodNodesHelper(root, root.val);
    }

    public int goodNodesHelper(TreeNode root, int max){
        
        //if root is null, it's not a "good node"
        if(root == null) return 0;

        //if the current value is more than the max till now, we have got a good node
        //+1 and check in LST and RST
        //also update the max to current node since that is the new max
        if(root.val >= max){
            return 1 + goodNodesHelper(root.left, root.val) + goodNodesHelper(root.right, root.val);
        //else, the current node is not a good node since it is lower than the max
        //so, keep checking with the same max in LST and RST
        }else{
            return goodNodesHelper(root.left, max) + goodNodesHelper(root.right, max);
        }

    }

}
```
