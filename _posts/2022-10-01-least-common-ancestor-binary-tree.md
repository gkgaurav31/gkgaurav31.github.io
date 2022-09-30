---
layout: post
title: Least Common Ancestor - Binary Tree
date: 2022-10-01 00:56 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Find the lowest common ancestor in an unordered binary tree A, given two values, B and C, in the tree.
[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### SOLUTION

- Get root to node path: [YouTube](https://www.youtube.com/watch?v=fmflMqVOC7k)
- In the previous step, there is a chance that one of the Node is not present, return null in that case
- Compare the paths stored in two lists to find the closest one.

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        if(root == null) return null;

        List<TreeNode> listA = new ArrayList<>();
        boolean checkA = solve(root, p, listA);

        List<TreeNode> listB = new ArrayList<>();
        boolean checkB = solve(root, q, listB);

        if(!checkA || !checkB) return null;

        return common(listA, listB);
    }
    
    public boolean solve(TreeNode A, TreeNode k, List<TreeNode> list){

        if(A == null) return false;

        list.add(A);

        if(A.val == k.val) return true;

        if(solve(A.left, k, list) || solve(A.right, k, list)){
            return true;
        }

        list.remove(list.size()-1);

        return false;
    }

    public TreeNode common(List<TreeNode> l1, List<TreeNode> l2){

        int i=0;

        while(i<l1.size() && i<l2.size() && l1.get(i).val == l2.get(i).val){
            i++;
        }

        return l1.get(i-1);

    }
    
}
```
