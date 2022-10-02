---
layout: post
title: Lowest Common Ancestor of a Binary Search Tree
date: 2022-10-03 00:26 +0530
author: "Gaurav Kumar"
tags: "java BST interviewbit leetcode"
categories: "BST"
---

### PROBLEM DESCRIPTION

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).  
[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

### SOLUTION

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        TreeNode t = root;
        
        while(t != null){

            //If both nodes has greater value, then the LCA must be on the RST
            if(p.val > t.val && q.val > t.val){
                t = t.right;

            //Else if both nodes have lesser value, LCA must be on LST
            }else if(p.val < t.val && q.val < t.val){
                t = t.left;

            //If one node has greater and other has smaller than the current node, then the current node must be the LCA
            }else{
                return t;
            }
        }
        
        return null;
        
    }
}
```
