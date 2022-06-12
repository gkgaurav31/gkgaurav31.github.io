---
layout: post
title: Symmetric Binary Tree
date: 2022-06-13 00:46 +0530
author: "Gaurav Kumar"
tags: "java BST"
categories: "leetcode"
---

### PROBLEM DESCRIPTION: [Symmetric Binary Tree](https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/536/)

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

### SOLUTION

```java
class Solution {
    
    public boolean isSymmetric(TreeNode root) {       
        if(root == null) return false;
        return isSym(root.left, root.right);
    }
    
    public boolean isSym(TreeNode left, TreeNode right){

        if(left == null && right == null){
            return true;
        } else if(left == null && right != null){
            return false;
        } else if(left != null && right == null){
            return false;
        }
        
        if (left.val != right.val){
            return false;
        }
        
        return isSym(left.left, right.right) && isSym(left.right, right.left);
    }

}

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
```
