---
layout: post
title: Kth smallest element in BST
date: 2022-10-02 13:40 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Find the lowest common ancestor in an unordered binary tree A, given two values, B and C, in the tree.
[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### SOLUTION

### APPROACH 1

Store the inorder traversal in a list and return (k-1)th element from the list.

```java
public class Solution {

    public int kthsmallest(TreeNode A, int B) {
        List<Integer> list = new ArrayList<>();
        inorder(A, list);
        return list.get(B-1);
    }

    public void inorder(TreeNode A, List<Integer> in){

        if(A == null) return;

        inorder(A.left, in);
        in.add(A.val);
        inorder(A.right, in);

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
