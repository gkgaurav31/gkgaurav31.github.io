---
layout: post
title: Invert Binary Tree
date: 2022-06-21 23:21 +0530
author: "Gaurav Kumar"
tags: "java binarytree"
categories: "algoexpert"
---

### PROBLEM DESCRIPTION

Invert the binary tree by swapping every every left node in the tree with its right node.

### SOLUTION

```java
import java.util.*;

class Program {
  public static void invertBinaryTree(BinaryTree tree) {

    if(tree == null) return;

    BinaryTree temp = tree.left;
    tree.left = tree.right;
    tree.right = temp;
    
    invertBinaryTree(tree.left);
    invertBinaryTree(tree.right);
 
  }

  static class BinaryTree {
    public int value;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int value) {
      this.value = value;
    }
  }
}
```

### Another way to code

```java
import java.util.*;

class Program {
  public static void invertBinaryTree(BinaryTree tree) {
    invertHelper(tree);
  }

  public static BinaryTree invertHelper(BinaryTree tree){
    
    if(tree == null) return null;

    BinaryTree temp = tree.left;
    
    tree.left = invertHelper(tree.right);
    tree.right = invertHelper(temp);

    return tree;
  }
}
```
