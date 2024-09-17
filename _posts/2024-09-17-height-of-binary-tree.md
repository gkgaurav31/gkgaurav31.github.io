---
layout: post
title: Height of Binary Tree (geeksforgeeks - SDE Sheet)
date: 2024-09-17 19:05 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree, find its height.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/height-of-binary-tree/1?page=7)

## SOLUTION

```java
class Solution {

    int height(Node node)
    {
        return node == null ? 0 : 1 + Math.max(height(node.left), height(node.right));
    }

}
```
