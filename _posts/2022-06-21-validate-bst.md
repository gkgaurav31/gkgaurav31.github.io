---
layout: post
title: Validate BST
date: 2022-06-21 20:08 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "algoexpert"
---

### PROBLEM DESCRIPTION

Given a tree, check whether it's a BST.

### SOLUTION

#### APPROACH 1

> MaxOf(Left Sub Tree) < Node.value <= MinOf(Right Sub Tree)

Using this relation, for each Node, we can find the max on LST and min on RST and check if the current Node belongs to that range. Recursively do this for each Node.

```java
import java.util.*;

class Program {
  
  public static boolean validateBst(BST tree) {

    if(tree == null) return true;
    boolean check = (tree.value > findMax(tree.left) && tree.value <= findMin(tree.right));
    return check && validateBst(tree.left) && validateBst(tree.right);
  }

  public static int findMax(BST tree){
    if(tree == null) return Integer.MIN_VALUE;
    int left = findMax(tree.left);
    int right = findMax(tree.right);

    if(tree.value > left && tree.value > right){
      return tree.value;
    }else if(left > right){
      return left;
    }else{
      return right;
    }
  }

  public static int findMin(BST tree){
    if(tree == null) return Integer.MAX_VALUE;
    int left = findMin(tree.left);
    int right = findMin(tree.right);

    if(tree.value < left && tree.value < right){
      return tree.value;
    }else if(left < right){
      return left;
    }else{
      return right;
    }
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
}
```

#### APPROACH 2

A more optimal way is to maintain the min and max possible values while making the recursive call. For the root node, it's going to be between INTEGER.MIN_VALUE & INTEGER.MAX_VALUE. When we call root.left, the possible value should be between (INTEGER.MIN_VALUE, root.value-1). For root.right: (root.value, INTEGER.MAX_VALUE). Assuming that the duplicate values are on the right sub-tree. This way we can keep doing the recursive call and updating the range of possible values. If the node.value does not fall in this range, return false.

```java
import java.util.*;

class Program {

  public static boolean validateBst(BST tree, int min, int max) {

    if(tree == null) return true;
    
    if(tree.value < min || tree.value > max) return false;

    boolean left = validateBst(tree.left, min, tree.value-1);
    boolean right = validateBst(tree.right, tree.value, max);

    return left && right;
    
  }
  
  public static boolean validateBst(BST tree) {
    return validateBst(tree, Integer.MIN_VALUE, Integer.MAX_VALUE);
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
    }
  }
}
```
