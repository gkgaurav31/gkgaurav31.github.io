---
layout: post
title: Flatten Binary Tree to Linked List
date: 2023-08-11 22:23 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcode150 important"
categories: "binarytree"
---
## PROBLEM DESCRIPTION

Given the root of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same TreeNode class where the right child pointer points to the next node in the list and the left child pointer is always null.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.

[leetcode](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## SOLUTION

### APPROACH 1 (EXTRA SPACE)

This problem is simple if we are allowed to use extra space because we can simple perform a pre-order traversal of the nodes and store them in a list. Then, all we need to do is to iterate over that list and update the left and right pointers for each node. The right pointer will simply be the next node in the list.

```java
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
class Solution {

    public void flatten(TreeNode root) {

        // Create a list to store the nodes in pre-order traversal order
        List<TreeNode> list = preOrder(root, new ArrayList<>());

        // Traverse through the list and modify the pointers to create the "linked list"
        for(int i=0; i<list.size()-1; i++){

            list.get(i).right = list.get(i+1); // Set the current node's right pointer to the next node
            list.get(i).left = null; // Set the left pointer to null

        }
    }

    public List<TreeNode> preOrder(TreeNode root, List<TreeNode> list){

        // Base case: If the current node is null, return the list
        if(root == null) return list;

        // Add the current node to the list
        list.add(root);
        // Recursively traverse the left subtree
        preOrder(root.left, list);
        // Recursively traverse the right subtree
        preOrder(root.right, list);

        return list; 
    }
}
```

### APPROACH 2 (RECURSION)

```java
class Solution {

    // The main function to flatten the binary tree
    public void flatten(TreeNode root) {
        // Call the helper function to perform the flattening
        root = flattenHelper(root);
    }

    // Helper function to recursively flatten the binary tree
    public TreeNode flattenHelper(TreeNode root) {

        // Base case: If the current node is null, return null
        if (root == null) return null;

        // Store the left and right subtrees of the current node
        TreeNode tempLST = root.left;
        TreeNode tempRST = root.right;

        // Clear the left and right pointers of the current node
        root.right = null;
        root.left = null;

        // Recursively flatten the left subtree
        TreeNode flatLST = flattenHelper(tempLST);
        
        // Recursively flatten the right subtree
        TreeNode flatRST = flattenHelper(tempRST);

        // Connect the flattened left subtree to the current node's right pointer
        root.right = flatLST;

        // Find the tail (last node) of the flattened left subtree (it's not attached to the root and we need to "append" the pre-order of right sub tree)
        TreeNode tail = root;
        while (tail.right != null) {
            tail = tail.right;
        }
        // Connect the tail of the flattened left subtree to the flattened right subtree
        tail.right = flatRST;

        // Return the root of the current flattened subtree
        return root;
    }
}
```
