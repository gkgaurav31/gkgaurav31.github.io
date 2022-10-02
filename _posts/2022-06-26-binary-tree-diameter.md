---
layout: post
title: Binary Tree Diameter
date: 2022-06-26 23:12 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "algoexpert"
---

### PROBLEM DESCRIPTION

Return the diameter of a given binary tree. Diameter is defined as the length of its longest path, even if that path doesn't pass through the node of the tree.

### SOLUTION

This is an interesting question because we need to maintain two information while traversing the binary tree. The obvious one is the sum of left path and the right path of any given node. If it's greater than the current max, then update the max value. (We are using a global variable here, but it's not the best practice.) For the next node that we need to check, it will need the maximum value between the length of it's child's left sub tree and child's right sub tree.  

![snapshot]({{ site.baseurl }}/assets/img/diameter.jpg)

```java
import java.util.*;

class Program {

  int ans = 0;
  
  public int binaryTreeDiameter(BinaryTree tree) {
    binaryTreeDiameterHelper(tree);
    return ans;
  }

  public int binaryTreeDiameterHelper(BinaryTree tree){
    
    if(tree == null)
      return 0;
    
    int left = binaryTreeDiameterHelper(tree.left);
    int right = binaryTreeDiameterHelper(tree.right);

    ans = Math.max(ans, left + right);
    return  (Math.max(left, right)+1);
    
  }
}
```

### SECOND APPROACH

```java
public class Solution {

    int maxDiameter = Integer.MIN_VALUE;

    public int solve(TreeNode A) {
        if(A == null) return 0;
        height(A);
        return maxDiameter;
    }

    int height(TreeNode node) 
    {
        if(node == null) return -1;
        
        int h1 = height(node.left);
        int h2 = height(node.right);
        maxDiameter = Math.max(maxDiameter, h1 + h2 + 2);
        return 1 + Math.max(h1, h2);
    }

}
```
