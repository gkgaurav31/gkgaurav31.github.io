---
layout: post
title: Lowest Common Ancestor - Using In-time and Out-time
date: 2022-10-03 01:35 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).  
[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/lca_using_intime_outtime.png)

```java
class Solution {
    
    Map<TreeNode, Integer> intime = new HashMap<>();
    Map<TreeNode, Integer> outtime = new HashMap<>();
    
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        //populate the in-time and out-time hash maps
        travel(root); 
                
        //start from root, which will be the ancestor of every node in the tree
        TreeNode current = root;
        
        while(current != null){

            //if the left node of current node is ancestor of both p and q, go left
            if(isAncestor(current.left, p) && isAncestor(current.left, q)){
                current = current.left;
            //if the right node of current node is ancestor of both p and q, go right
            }else if(isAncestor(current.right, p) && isAncestor(current.right, q)){
                current = current.right;
            //otherwise, current node must be the LCA
            }else{
                return current;
            }
        }
        
        return null;
        
    }
    
    int T = 0; 
    public void travel(TreeNode node){
        if(node == null) return;
        intime.put(node, T); T++;
        travel(node.left);
        travel(node.right);
        outtime.put(node, T); T++;
    }
    
    //Is A ancestor of B
    public boolean isAncestor(TreeNode A, TreeNode B){
        
        if(A == null || B == null) return false;
        
        int inA = intime.get(A);
        int inB = intime.get(B);
        int outA = outtime.get(A);
        int outB = outtime.get(B);
        
        //Main logic to check if A is an ancestor of B
        if(inA <= inB && outA >= outB){
            return true;
        }
        
        return false;
    }
}
```
