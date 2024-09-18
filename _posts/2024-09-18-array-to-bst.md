---
layout: post
title: Array to BST (geeksforgeeks - SDE Sheet)
date: 2024-09-18 21:58 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sorted array. Convert it into a Height Balanced Binary Search Tree (BST). Return the root of the BST.

Height-balanced BST means a binary tree in which the depth of the left subtree and the right subtree of every node never differ by more than 1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/array-to-bst4443/1?page=8)

## SOLUTION

We can solve this by using a recursive approach to convert a sorted array into a balanced Binary Search Tree (BST). The key idea is to select the middle element of the array (or subarray) as the root of the BST, ensuring that the tree remains balanced.

The middle element splits the array into two halves: the left half is used to recursively build the left subtree, and the right half is used for the right subtree. The base case of the recursion occurs when the left index exceeds the right index, meaning there are no elements to process, so we return null.

```java
class Solution {

    public Node sortedArrayToBST(int[] nums) {
        return createTree(nums, 0, nums.length - 1);
    }

    public Node createTree(int[] nums, int l, int r) {

        // Base case: if left index exceeds right index, return null (no node)
        if (l > r)
            return null;

        // Find the middle element to be the root node of the current subtree
        int mid = (l + r) / 2;

        // Create a new node with the middle element
        Node node = new Node(nums[mid]);

        // Recursively build the left subtree using the left half of the array
        node.left = createTree(nums, l, mid - 1);

        // Recursively build the right subtree using the right half of the array
        node.right = createTree(nums, mid + 1, r);

        // Return the created node (root of the current subtree)
        return node;
    }

}
```
