---
layout: post
title: Lowest Common Ancestor of a Binary Tree
date: 2023-08-12 22:32 +0530
author: "Gaurav Kumar"
tags: "java BST leetcode leetcode150"
categories: "BST"
---
## PROBLEM DESCRIPTION

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## SOLUTION (NOT OPTIMAL)

```text
       ____3____
      /         \
   __5__        _1__
  /     \      /    \
 6       2    0      8
        / \
       7   4
```

> Inorder traversal of the tree is: 6 __5__ 7 2 4 __3__ 0 __1__ 8  

Given:  
p = 5 and q = 1  

3 is the root element in this case. We notice that index of p is less than that, and index of q is more. That means, p and q must be lying of different side of the current node and it can be marked as LCA.  

If both p and q were on left side of current root, we can update root to root.left.  
If both p and q were on right side of current root, we can update root to root.right.  

If can happen can current root is equal to either p or q. (For example, if p is a child of q). In that case, the current root, which would be equal to either p or q can be marked as LCA. We can use this condition in a while loop.

```java
class Solution {
    
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Perform an in-order traversal of the tree to get the list of nodes in in-order sequence
        List<TreeNode> inorder = inorder(root, new ArrayList<>());

        TreeNode current = root; // Start traversal from the root node

        int pIndex = getIndex(inorder, p); // Get the index of node p in the in-order list
        int qIndex = getIndex(inorder, q); // Get the index of node q in the in-order list

        int rootIndex = getIndex(inorder, current); // Get the index of the current root in the in-order list

        // Traverse the tree until the root node's index matches either p's index or q's index
        while (rootIndex != pIndex && rootIndex != qIndex) {
            if (pIndex < rootIndex && qIndex < rootIndex) {
                current = current.left; // Move to the left child if both p and q are in the left subtree
                rootIndex = getIndex(inorder, current); // Update the current root's index
            } else if (pIndex > rootIndex && qIndex > rootIndex) {
                current = current.right; // Move to the right child if both p and q are in the right subtree
                rootIndex = getIndex(inorder, current); // Update the current root's index
            } else {
                return current; // If p and q are on different subtrees, the current node is the LCA
            }
        }

        return current; // Return the lowest common ancestor
    }

    // Get the index of a node in the in-order list
    public int getIndex(List<TreeNode> inorder, TreeNode current) {
        for (int i = 0; i < inorder.size(); i++) {
            if (inorder.get(i) == current) return i; // Return the index when the node is found
        }
        return -1; // Return -1 if the node is not found
    }

    // Perform an in-order traversal of the tree and populate the in-order list
    public List<TreeNode> inorder(TreeNode root, List<TreeNode> list) {
        if (root == null) return list; // Base case: Return the list when the node is null

        inorder(root.left, list); // Traverse left subtree
        list.add(root); // Add the current node to the list
        inorder(root.right, list); // Traverse right subtree

        return list;
    }
}
```

For more optimal solutions, check this out:  

- [Lowest Common Ancestor - Using In-time and Out-time]({% post_url 2022-10-03-lowest-common-ancestor-using-intime-and-outtime %})
- [Lowest Common Ancestor of a Binary Search Tree]({% post_url 2022-10-03-lowest-common-ancestor-of-a-binary-search-tree %})
