---
layout: post
title: Binary Search Tree Iterator
date: 2023-08-12 21:44 +0530
author: "Gaurav Kumar"
tags: "java BST leetcode leetcode150"
categories: "BST"
---
## PROBLEM DESCRIPTION

Implement the BSTIterator class that represents an iterator over the in-order traversal of a binary search tree (BST):

-BSTIterator(TreeNode root) Initializes an object of the BSTIterator class. The root of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
-boolean hasNext() Returns true if there exists a number in the traversal to the right of the pointer, otherwise returns false.
-int next() Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to next() will return the smallest element in the BST.

You may assume that next() calls will always be valid. That is, there will be at least a next number in the in-order traversal when next() is called.

[leetcode](https://leetcode.com/problems/binary-search-tree-iterator/)

## SOLUTION

The idea is to perform inorder traversal of the BST and save it in a list. Use a "position" variable to track the current index.

```java
class BSTIterator {

    // The root of the BST
    TreeNode root;          
    // List to store the in-order traversal
    List<TreeNode> inorder;

    // Current position in the inorder list
    private int position;  

    public BSTIterator(TreeNode root) {
        this.root = root; // Initialize the root of the BST
        this.inorder = inorder(root, new ArrayList<TreeNode>()); // Create the in-order traversal list
    }

    // Recursive function to perform in-order traversal and populate the list
    public List<TreeNode> inorder(TreeNode root, List<TreeNode> inorder){
        if(root == null) return inorder; 

        inorder(root.left, inorder);     
        inorder.add(root);               
        inorder(root.right, inorder);   

        return inorder;
    }
    
    // Return the value of the node at the current position and move the position forward
    public int next() {
        return inorder.get(position++).val; 
    }
    
    // Check if there are more nodes to iterate over
    // If the position has reached the end of the list, return false
    public boolean hasNext() {
        if(position <= inorder.size() - 1) return true; 
        return false; 
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```
