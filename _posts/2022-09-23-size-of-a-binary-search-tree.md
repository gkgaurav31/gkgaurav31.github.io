---
layout: post
title: Size of a Binary Search Tree
date: 2022-09-23 00:13 +0530
author: "Gaurav Kumar"
tags: "java BST"
categories: "BST"
---

### PROBLEM DESCRIPTION

Given a binary tree, find its size (Total number of Nodes in the tree).

### SOLUTION

```java
class Tree
{
    public static int getSize(Node root)
    {
        if(root == null) return 0;
        
        int s1 = getSize(root.left);
        int s2 = getSize(root.right);
        return 1 + s1 + s2;
    }
}
```
