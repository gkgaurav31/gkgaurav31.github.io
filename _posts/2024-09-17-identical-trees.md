---
layout: post
title: Identical Trees (geeksforgeeks - SDE Sheet)
date: 2024-09-17 19:09 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two binary trees, the task is to find if both of them are identical or not.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/determine-if-two-trees-are-identical/1?page=7)

## SOLUTION

```java
class Solution
{

    boolean isIdentical(Node root1, Node root2)
    {

        if(root1 == null && root2 == null)
            return true;

        if(root1 == null || root2 == null)
            return false;

        if(root1.data != root2.data)
            return false;

        return isIdentical(root1.left, root2.left) && isIdentical(root1.right, root2.right);

    }

}
```
