---
layout: post
title: Valid Binary Search Tree
date: 2022-09-27 00:11 +0530
author: "Gaurav Kumar"
tags: "java BST important"
categories: "BST"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree, determine if it is a valid binary search tree (BST).  

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/valid_bst.png)

```java
public class Solution {
    public int isValidBST(TreeNode A) {
        return isValidBSTHelper(A, Integer.MIN_VALUE, Integer.MAX_VALUE)?1:0;
    }

    public boolean isValidBSTHelper(TreeNode root, int start, int end){
        if(root == null) return true;
        boolean valid = (root.val >= start && root.val <= end);
        return valid && isValidBSTHelper(root.left, start, root.val-1) && isValidBSTHelper(root.right, root.val+1, end);
    }

}
```
