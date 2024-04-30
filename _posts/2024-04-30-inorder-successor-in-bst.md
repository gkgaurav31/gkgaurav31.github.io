---
layout: post
title: Inorder Successor in BST
date: 2024-04-30 16:34 +0530
author: "Gaurav Kumar"
tags: "java binarytree binary_search"
categories: "binarytree"
---

## PROBLEM DESCRIPTION

Given the root of a binary search tree and a node `p` in it, return the in-order successor of that node in the BST. If the given node has no in-order successor in the tree, return `null`.

The successor of a node p is the node with the smallest key greater than `p.val`.

[leetcode](https://leetcode.com/problems/inorder-successor-in-bst/description/)

## SOLUTION

### APPROACH 1

We can create a min heap to store all nodes which have values more than `p.val`. The top most element will be the successor because the inorder has all sorted elements in a BST.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {

        // min heap
        Queue<TreeNode> heap = new PriorityQueue<>((a, b) -> a.val - b.val);

        // in-order traversal
        inorder(root, p, heap);

        // if heap is empty, p must be the last visited node
        // otherwise, the least element which is more than p.val must be the successor. This will be stored at the top of min heap (we have only added node to min heap if its value is more than p.val)
        return (heap.size() == 0) ? null : heap.peek();

    }

    public Queue<TreeNode> inorder(TreeNode root, TreeNode p, Queue<TreeNode> heap){

        // return if root is null
        if(root == null)
            return heap;

        // look into left sub tree
        inorder(root.left, p, heap);

        // if current node value is more than p, add to heap
        if(root.val > p.val){
            heap.add(root);
        }

        // look into right sub tree
        inorder(root.right, p, heap);

        return heap;

    }

}
```
