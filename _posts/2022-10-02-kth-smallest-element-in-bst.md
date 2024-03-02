---
layout: post
title: Kth Smallest Element in a BST
date: 2022-10-02 13:40 +0530
author: "Gaurav Kumar"
tags: "java binarytree neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

### SOLUTION

### APPROACH 1

Store the inorder traversal in a list and return (k-1)th element from the list.

```java
class Solution {

    public int kthSmallest(TreeNode root, int k) {
        return inorder(root, new ArrayList<>()).get(k-1);
    }

    public List<Integer> inorder(TreeNode root, List<Integer> list){

        if(root == null)
            return list;

        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);

        return list;

    }

}
```

### APPROACH 2 (Optimized)

We don't need to traversal the entire list necessarily. Once we reach the Kth element, we should be returning it. This will be O(k) time complexity.

```java
public class Solution {

    public int kthsmallest(TreeNode A, int B) {

        //To store the value of K
        int[] k = new int[1];
        k[0] = B;

        TreeNode n = solve(A, k);

        //In case K is more than the number of nodes
        if(n != null)
            return n.val;
        return -1;

    }

    public TreeNode solve(TreeNode root, int[] k){

        if(root == null) return null;

         // We do an inorder traversal here.
        TreeNode left = solve(root.left, k);
        if (k[0] == 0)
            return left;    // left subtree has k or more elements.
        k[0]--;
        if(k[0] == 0){
            return root; // root is the kth element.
        }
        return solve(root.right, k);  // answer lies in the right node.
    }
}
```
