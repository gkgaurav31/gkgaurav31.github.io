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
[leetcode](https://leetcode.com/problems/validate-binary-search-tree/)

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

The above code may not work if the value in the Nodes are itself Integer.MIN_VALUE or Integer.MAX_VALUE. (Because, in the recursive call, we will add or subtract 1 which will exceed the limit for the Integer value)

We can instead pass null as start and end limit, and check the range only when start/end is not null:

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBSTHelper(root, null, null);
    }
    
    public boolean isValidBSTHelper(TreeNode root, Long start, Long end){
        if(root == null) return true;
        boolean valid=true;
        if(start != null)
            if(root.val < start) valid = false;
        if(end != null)
            if(root.val > end) valid = false;
        return valid && isValidBSTHelper(root.left, start, (long)root.val-1) && isValidBSTHelper(root.right, (long)root.val+1, end);
    }
}
```
