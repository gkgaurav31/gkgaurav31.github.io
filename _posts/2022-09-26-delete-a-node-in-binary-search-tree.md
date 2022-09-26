---
layout: post
title: Delete a Node in Binary Search Tree
date: 2022-09-26 23:04 +0530
author: "Gaurav Kumar"
tags: "java BST important"
categories: "BST"
---

### PROBLEM DESCRIPTION

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

[leetcode](https://leetcode.com/problems/delete-node-in-a-bst/submissions/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/delete_node_bst.png)

```java
class Solution {
    
    public TreeNode deleteNode(TreeNode root, int key) {
        
        if(root == null) return null;
        
        if(root.val == key){
            
            //No child
            if(root.left == null && root.right == null){
                return null;
            }
            
            //Only left child
            if(root.left == null && root.right != null){
                return root.right;
            }
            
            //Only right child
            if(root.left != null && root.right == null){
                return root.left;                
            }
            
            //Two children
            int max = getMaxValueInBST(root.left);
            root.val = max;
            root.left = deleteNode(root.left, max);
            
        }
        
        if(key < root.val){
            root.left = deleteNode(root.left, key);
        }
        
        if(key > root.val){
            root.right = deleteNode(root.right, key);
        }
        
        return root;
        
    }
    
    public int getMaxValueInBST(TreeNode root){
        while(root.right != null){
            root = root.right;
        }
        return root.val;
    }
    
}
```
