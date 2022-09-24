---
layout: post
title: Height of a Binary Search Tree
date: 2022-09-23 00:03 +0530
author: "Gaurav Kumar"
tags: "java binarytree"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given a binary tree, find its height.

### SOLUTION

Here we are considering the height of leaf node to be 0. To handle the edge case, it is important to return -1 when the node is null.

> if(node == null) return -1;  

```java
class Solution {

    //Function to find the height of a binary tree.
    //Height of the leaf node is 0 -> Edge Case
    int height(Node node) 
    {
        if(node == null) return -1;
        
        int h1 = height(node.left);
        int h2 = height(node.right);
        return 1 + Math.max(h1, h2);
    }
}
```
