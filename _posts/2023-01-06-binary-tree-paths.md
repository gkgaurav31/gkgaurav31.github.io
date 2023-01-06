---
layout: post
title: Binary Tree Paths
date: 2023-01-06 16:08 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode backtracking"
categories: "backtracking"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree, return all root-to-leaf paths in any order.
A leaf is a node with no children.

[Leetcode](https://leetcode.com/problems/binary-tree-paths/)

### SOLUTION

```java
class Solution {
    
    public List<String> binaryTreePaths(TreeNode root) {
        pathHelper(root, new ArrayList<String>());
        return convertToListOfString(ans);
    }

    List<List<String>> ans = new ArrayList<List<String>>();

    public void pathHelper(TreeNode root, List<String> list){

        if(root == null){
            return;
        }

        //pre-order traversal
        list.add(root.val + "");

        pathHelper(root.left, list);
        pathHelper(root.right, list);

        //if leaf node, add to anser list
        if(root.left == null && root.right == null){
            ans.add(new ArrayList<String>(list));
        }

        //backtrack
        list.remove(list.size()-1);

    }

    public List<String> convertToListOfString(List<List<String>> list){
        
        List<String> ans = new ArrayList<>();

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<list.size(); i++){

            List<String> t = list.get(i);

            for(int j=0; j<t.size(); j++){
                sb.append(t.get(j) + "->");
            }

            ans.add(new String(sb.toString().substring(0, sb.length()-2)));

            sb = new StringBuffer();

        }

        return ans;

    }

}
```
