---
layout: post
title: Morris Traversal
date: 2022-10-02 22:43 +0530
author: "Gaurav Kumar"
tags: "java binarytree important geeksforgeeks"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Binary Tree traversal using Morris Algorithm. Iterative approach, no stack required - O(1) space complexity.
[geeksforgeeks](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion-and-without-stack/)

### SOLUTION

Using Morris Traversal, we can traverse the tree without using stack and recursion. The idea of Morris Traversal is based on Threaded Binary Tree. In this traversal, we first create links to Inorder successor and print the data using these links, and finally revert the changes to restore original tree.  

![snapshot]({{ site.baseurl }}/assets/img/morris_inorder_traversal.png)

```java
class Solution {
    
    public List<Integer> inorderTraversal(TreeNode root) {
        
        TreeNode current = root;
        
        List<Integer> inorder = new ArrayList<>();
        
        //While current is not NULL
        while(current != null){
            
            //If the current does not have left child
            if(current.left == null){
                // a) Print current’s data
                // b) Go to the right, i.e., current = current->right
                inorder.add(current.val);
                current = current.right;
                
            }else{
                
                //Else, get the predecessor (OR node whose right child == current.)
                TreeNode pred = getPredecessor(current);
                
                //If we found right child == current
                if(pred.right == current){
                    
                    //a) Update the right child as NULL of that node whose right child is current
                    //b) Save current’s data
                    //c) Go to the right, i.e. current = current->right
                    pred.right = null;
                    inorder.add(current.val);
                    current = current.right;
                }else{
                    //a) Make current as the right child of that rightmost node we found
                    pred.right = current;
                    //b) Go to this left child, i.e., current = current->left
                    current = current.left;
                }

            }

        }
        
        return inorder;
        
    }
    
    //Get right most element in LST
    public TreeNode getPredecessor(TreeNode root){
        
        if(root.left == null) return null;
        
        TreeNode t = root.left;
        
        //t.right can be equal to root if we have already created a link to that element previously
        while(t.right != null && t.right != root){
            t = t.right;
        }
        
        return t;
        
    }
}
```
