---
layout: post
title: Binary Tree to DLL (geeksforgeeks - SDE Sheet)
date: 2024-09-14 22:16 +0530
author: "Gaurav Kumar"
tags: "java binarytree linkedlist geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Tree (BT), convert it to a Doubly Linked List (DLL) in place. The left and right pointers in nodes will be used as previous and next pointers respectively in converted DLL. The order of nodes in DLL must be the same as the order of the given Binary Tree. The first node of Inorder traversal (leftmost node in BT) must be the head node of the DLL.

Note: h is the tree's height, and this space is used implicitly for the recursion stack.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/binary-tree-to-dll/1?page=7)

## SOLUTION

### APPROACH 1

The first approach is straight forward, when extra space is allowed. Save the inorder traversal in a list. Then, keep iterating to all the nodes in the list and update their left and right pointers to the previous and next nodes in the list.

```java
class Solution
{
    //Function to convert binary tree to doubly linked list and return it.
    Node bToDLL(Node root)
    {

        if(root == null)
            return null;

        List<Node> list = inorder(root, new ArrayList<>());

        for(int i=0; i<list.size(); i++){

            if(i > 0) list.get(i).left = list.get(i-1);
            if(i < list.size()-1) list.get(i).right = list.get(i+1);

        }

        return list.get(0);

    }

    List<Node> inorder(Node root, List<Node> list){

        if(root == null)
            return list;

        inorder(root.left, list);
        list.add(root);
        inorder(root.right, list);

        return list;

    }

}
```

### APPROACH 2

```java
class Solution
{

    Node bToDLL(Node root)
    {
        // Base case: If the root is null, return null (this is for leaf nodes and empty subtrees)
        if(root == null)
            return null;

        // Recursively convert the left subtree to DLL
        Node flatLeft = bToDLL(root.left);

        // Initialize a pointer to traverse to the tail (rightmost node) of the left subtree's DLL
        Node tailLeft = flatLeft;

        // If the left subtree is not empty, find the rightmost node (tail) and link it to the current root
        if(tailLeft != null){
            // Traverse to the rightmost node of the left subtree's DLL
            while(tailLeft.right != null)
                tailLeft = tailLeft.right;

            // Connect the current root to the tail of the left subtree
            tailLeft.right = root; // Tail's next (right) points to the root

            root.left = tailLeft;  // Root's previous (left) points to the tail

        }

        // Recursively convert the right subtree to DLL
        Node flatRight = bToDLL(root.right);

        // Link the current root to the head of the right subtree's DLL
        root.right = flatRight; // Root's next (right) points to the right subtree's head

        // If the right subtree is not empty, set its previous (left) pointer to the root
        if(flatRight != null){

            // Right subtree's head's previous points to the root
            flatRight.left = root;
        }

        // Return the head of the newly formed DLL
        // If the left subtree exists, the head is the leftmost node, otherwise, it's the root
        return flatLeft != null ? flatLeft : root;
    }
}
```
