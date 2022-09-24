---
layout: post
title: Construct Binary Tree from Preorder and Inorder Traversal
date: 2022-09-25 00:56 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree. (preorder and inorder consist of unique values.)
[Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### SOLUTION

```java
class Solution {
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return reconstructBstHelper(preorder, inorder, 0, preorder.length-1, 0, inorder.length-1);
    }
    
    public TreeNode reconstructBstHelper(int[] preorder, int[] inorder, int ps, int pe, int ins, int ine) {

        //Base Condition
        if(ps > pe || ins > ine) return null;

        int rootVal = preorder[ps];
        TreeNode n = new TreeNode(rootVal);

        int idx = getIndexOfRootInOrder(inorder, rootVal);

        int lengthOfLST = idx-ins;

        //LST
        n.left = reconstructBstHelper(preorder, inorder, ps+1, ps+lengthOfLST, ins, idx-1);

        //RST
        n.right = reconstructBstHelper(preorder, inorder, ps+lengthOfLST+1, pe, idx+1, ine);

        return n;

    }

    public int getIndexOfRootInOrder(int[] list, int val){

        for(int i=0; i<list.length; i++){
          if(list[i] == val) return i;
        }

        return -1;
    }

}
```
