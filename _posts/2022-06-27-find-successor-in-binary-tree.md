---
layout: post
title: Find Successor in Binary Tree
date: 2022-06-27 01:21 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

We are given a tree in which every Node has another attribute called parent which points to its immediate ancestor. We are also given another Node as input, which is present in that binary tree. We need to return the next Node which will be visited as per inorder traversal, after the input Node.

### SOLUTION

The brute force approach is to create a list of elements visited in inorder. Then loop through the list and return the next node which matches with the input Node given.

A more optimal approach involves making the following observations:

- Case 1: If there is a RST for the input Node, then the next element to be visited will be the left most element in the RST of that Node. Example: The input Node is 2. As per inorder, right node must be visited next. However, in Node 5, before visiting that Node, it will need to tranverse toward its left most element first. If there was another element 7 present, that will be the successor.
  
- Case 2: If the Node does not have any right sub tree, then we will need to look at the ancestors. The inorder traversal is: LEFT, VISIT, RIGHT. So, if the current element is the right child of its ancestor, we are sure that its ancestor has already been visited. We check the same thing for the ancestor. If the ancestor again is the right child of its ancestor, we can say that that has also been visited. So, we keep looking at all the ancestors until we find that the current Node is the left child of its ancestor. In that case, we just return the ancestor as our next node to be visited. Example: If the input Node is 5, which does not have any RST, we will need to look at its ancestors. Node 5 is the right child of its ancestor. So we can say that the ancestor was already visited (since order is: LEFT, VISIT, RIGHT). Then we check the ancestor of Node 2, which is Node 1. Node 2 is the left child of Node 1. So, the next successor must be Node 1. If we followed the same process for Node 3, we would need to check the ancestor of root element. That means, Node 3 does not have any successor.

![snapshot]({{ site.baseurl }}/assets/img/successor_binary_tree.jpg)

```java
  public BinaryTree getLeftMostElementInRST(BinaryTree node){
    while(node.left != null) node = node.left;
    return node;
  }

  public BinaryTree findSuccessor(BinaryTree tree, BinaryTree node) {

    if(node.right != null){
      return getLeftMostElementInRST(node.right);
    }

    BinaryTree temp = node;
    
    while(temp.parent != null){
      BinaryTree parent = temp.parent;
      if(parent.left == temp) return parent;
      temp = parent;
    }

    return null;
    
  }
  ```
