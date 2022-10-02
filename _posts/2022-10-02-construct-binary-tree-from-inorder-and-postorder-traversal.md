---
layout: post
title: Construct Binary Tree from Inorder and Postorder Traversal
date: 2022-10-02 21:22 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given two integer arrays inorder and postorder where inorder is the inorder traversal of a binary tree and postorder is the postorder traversal of the same tree, construct and return the binary tree.  
[Leetcode](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### SOLUTION

```java
class Solution {
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return buildHelper(inorder, postorder, 0, inorder.length-1, 0, postorder.length-1);
    }
    
    //ins -> inorder start
    //ine -> inorder end
    //ps -> postorder start
    //pe -> postorder end
    public TreeNode buildHelper(int[] inorder, int[] postorder, int ins, int ine, int ps, int pe){
        
        if(ins > ine || ps > pe) return null;

        //Last element in the postorder is the root
        int r = postorder[pe];
        TreeNode node = new TreeNode(r);

        //Get index of the root element in inorder
        int idx = getRootInOrder(inorder, r, ins, ine);

        //Size of right subtree
        int sizeOfRST = ine - idx;

        //We can now assign right of this node to subtree which can be constructed using recusive call
        node.right = buildHelper(inorder, postorder, idx+1, ine, pe-sizeOfRST, pe-1);
        node.left = buildHelper(inorder, postorder, ins, idx-1, ps, pe-sizeOfRST-1);
        
        return node;

    }

    public int getRootInOrder(int[] inorder, int r, int start, int end){
        for(int i=start; i<=end; i++){
            if(inorder[i] == r) return i;
        }
        return -1;
    }
    
}
```
