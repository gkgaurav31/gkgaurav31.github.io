---
layout: post
title: Validate Three Nodes in BST
date: 2022-10-03 19:55 +0530
author: "Gaurav Kumar"
tags: "java BST"
categories: "BST"
---

### PROBLEM DESCRIPTION

You are given three Nodes of a BST - NodeOne, NodeTwo and NodeThree. Write a function that returns true if one of NodeOne and NodeThree is an ancestor of NodeTwo. The other Node must be decendent of NodeTwo. If this is not the case, return false.  

### SOLUTION

```java
  public boolean validateThreeNodes(BST nodeOne, BST nodeTwo, BST nodeThree) {

    if(checkIfAncestor(nodeOne, nodeTwo) && checkIfAncestor(nodeTwo, nodeThree)){
      return true;
    }else if(checkIfAncestor(nodeThree, nodeTwo) && checkIfAncestor(nodeTwo, nodeOne)){
      return true;
    }
    return false;
  }

  //Returns true if root is ancestor of k
  public boolean checkIfAncestor(BST root, BST k){
    if(root == null) return false;
    if(root.value == k.value) return true; //This will also work: if(root == k) return true;
    return checkIfAncestor(root.left, k) || checkIfAncestor(root.right, k);
    
  }
```
