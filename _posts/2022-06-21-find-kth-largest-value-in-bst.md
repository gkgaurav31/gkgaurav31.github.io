---
layout: post
title: Find kth Largest Value in BST
date: 2022-06-21 15:04 +0530
author: "Gaurav Kumar"
tags: "java linkedlist important"
categories: "algoexpert"
---

### PROBLEM DESCRIPTION

Given a BST, find the kth largest element in that BST.

### SOLUTION

The trivial way is to do a inorder traversal of the BST - which will be sorted. Then return the (n-k)th element from the inorder. (We would need additional space to store inorder as a list/array)  

Since we are looking for the larger elements, it would be better to get an inverse of inorder, which can be easily done if we just go to the right subtree first. We can then use some counter value to check if we are "visiting" the kth element. In that case we can return that value. For count>k, we can simply return without performing any action. This will reduce the number of iteration since we do not need to check any more elements in the tree.

```java
import java.util.*;

class Program {

  static class BST {
    public int value;
    public BST left = null;
    public BST right = null;

    public BST(int value) {
      this.value = value;
    }
  }

  int count=0;
  int max=0;
  
  public void reverseInOrder(BST tree, int k){
    if(tree == null || count>k) return; //if count > k, no need to visit any element so just return
    reverseInOrder(tree.right, k);
    count++;
    if(count == k) max = tree.value;
    reverseInOrder(tree.left, k);
  }

  public int findKthLargestValueInBst(BST tree, int k) {
    reverseInOrder(tree, k);
    return max;
  }
}
```

### Brute-Force

```java
import java.util.*;

class Program {

  static class BST {
    public int value;
    public BST left = null;
    public BST right = null;

    public BST(int value) {
      this.value = value;
    }
  }

  List<Integer> list = new ArrayList<Integer>();

  public void inorder(BST tree){

    if(tree == null) return;
    inorder(tree.left);
    list.add(tree.value);
    inorder(tree.right);
  }
  
  public int findKthLargestValueInBst(BST tree, int k) {
    inorder(tree);
    return list.get(list.size()-k);
  }
}
```
