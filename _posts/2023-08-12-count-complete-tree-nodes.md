---
layout: post
title: Count Complete Tree Nodes
date: 2023-08-12 19:04 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcode150 important"
categories: "binarytree"
---

## PROBLEM DESCRIPTION

Given the root of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.

[leetcode](https://leetcode.com/problems/count-complete-tree-nodes/)

## SOLUTION

### THE SIMPLE WAY

Use any of the ways to traverse the binary tree and calculate the number of nodes. However, this will have a time complexity of O(n).

```java
class Solution {

    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        return 1 + countNodes(root.left) + countNodes(root.right);

    }

}
```

### OPTIMIZED (USING BINARY SEARCH)

![snapshot]({{ site.baseurl }}/assets/img/count_complete_tree_nodes_part_1.png)
![snapshot]({{ site.baseurl }}/assets/img/count_complete_tree_nodes_part_2.png)

```text
Time Complexity: O(d^2) = O(logN)
where d is the depth of the tree.
```

```java
class Solution {
    
    public int countNodes(TreeNode root) {

        // Check if the root is null, meaning an empty tree
        if(root == null) return 0;

        // Calculate the depth of the leftmost path (height of the left subtree)
        int depth = calculateDepth(root);

        // If the tree is a perfect tree (all levels except the last are completely filled)
        if(depth == 0) return 1;

        // Calculate the number of nodes before the last level using the formula 2^d - 1
        int nodesBeforeLastLevel = (int)Math.pow(2, depth) - 1;

        // Calculate the maximum number of nodes that can be in the last level (between 1 and 2^d inclusive)
        int nodesLastLevel = (int)Math.pow(2, depth);

        // Initialize binary search boundaries for checking presence of leaf nodes
        int left = 0;
        int right = nodesLastLevel-1;

        int presentTillIndex = -1; // This variable will store the highest leaf index that is present

        // Perform binary search to find the highest leaf index that is present
        while(left <= right){
            int pivot = (left + right) / 2;

            // Check if the leaf node with index 'pivot' is present
            if(isLeafPresent(root, depth, nodesLastLevel, pivot)){
                presentTillIndex = Math.max(presentTillIndex, pivot);
                left = pivot + 1; // Move the left boundary to search for higher indices
            } else {
                right = pivot - 1; // Move the right boundary to search for lower indices
            }
        }

        // Return the total number of nodes in the tree
        return nodesBeforeLastLevel + presentTillIndex + 1;
    }

    // Check if a leaf node with a given index is present in the last level
    public boolean isLeafPresent(TreeNode root, int depth, int nodesLastLevel, int idx){
        int left = 0;
        int right = nodesLastLevel - 1;

        TreeNode t = root;

        // Traverse down the tree following the path indicated by the binary representation of idx
        for(int i = 0; i < depth; i++){
            int pivot = (left + right) / 2;

            // Move to the left child if the index is less than or equal to the pivot
            if(idx <= pivot){
                t = t.left;
                right = pivot - 1;
            } else {
                // Move to the right child if the index is greater than the pivot
                t = t.right;
                left = pivot + 1;
            }
        }

        // If 't' is not null, the leaf node at the given index is present
        return t != null;
    }

    // Calculate the depth of the leftmost path in the tree
    // We know that the given tree is a complete binary tree, so we can simply follow the left nodes
    public int calculateDepth(TreeNode root){
        int depth = 0;
        TreeNode t = root;

        // Traverse down the leftmost path while counting the levels
        while(t.left != null){
            depth++;
            t = t.left;
        }

        return depth;
    }
}
```
