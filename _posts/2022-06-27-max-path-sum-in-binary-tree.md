---
layout: post
title: Max Path Sum in Binary Tree
date: 2022-06-27 13:08 +0530
author: "Gaurav Kumar"
tags: "java binarytree"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given a binary tree, return its max path sum.

### SOLUTION

The apprach to this problem is similar to [Binary Tree Diameter]({% post_url 2022-06-26-binary-tree-diameter %}). We keep a track of two things, the sum in LST and the sum in RST, and return the max between them when calling the function recursively.

![snapshot]({{ site.baseurl }}/assets/img/max_path_sum_binary_tree.jpg)

```java
import java.util.*;

class Program {

  public static int maxSum = Integer.MIN_VALUE;
  
  public static int maxPathSumHelper(BinaryTree tree) {

    if(tree == null) return 0;
    
    int left = maxPathSumHelper(tree.left) + tree.value;
    int right = maxPathSumHelper(tree.right) + tree.value;

    //since tree.value has been added twice so we decrease it once to get the actual sum
    if(left+right-tree.value > maxSum){
      maxSum = left+right-tree.value;
    }
    
    //if sum on LST or RST can be -ve. So the previos block will actually give us a lower total sum. Hence, we need to check the sum only on the LST and only on the RST as well.
    if(left > maxSum){
      maxSum = left;
    }
    
    if(right > maxSum){
      maxSum = right;
    }

    //Important: we are not returning the total path sum, but rather the max between left and right sub tree
    return Math.max(left, right);
    
  }
  
  public static int maxPathSum(BinaryTree tree) {
    maxSum = Integer.MIN_VALUE;
    maxPathSumHelper(tree);
    return maxSum;
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
