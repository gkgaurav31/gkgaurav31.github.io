---
layout: post
title: Binary Tree - Level Order Traversal II
date: 2022-09-23 23:38 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
[Leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### SOLUTION

For the main explaination, see:
[BST Level Order Traversal]({% post_url 2022-06-12-binary-tree-level-order-traversal %})

This query needs some tweaking, because we need to return the levels as a List<List<Integer>>. So, after every level, we need a way to create a new List<Integer> to add to our response.

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        
        //If the BST is null
        if(root == null) return new ArrayList<List<Integer>>();
        
        //We will make the Queue type as TreeNode because we need to access the left and right children later
        Queue<TreeNode> q = new LinkedList<>();

        //Initialize the queue with the root node and NULL
        //The idea is to add NULL after every level
        //While removing elements from the queue, if we encounter a NULL, that means that we have covered a certain level
        //So we can append the current list to the answer and continue
        q.add(root);
        q.add(null);
        
        //To store final answer
        List<List<Integer>> res = new ArrayList<>();

        //To store each level
        List<Integer> temp = new ArrayList<>();
        
        //If there is one node and NULL, size must be at least 1
        while(q.size() > 1){

            //Remove the element from the queue
            TreeNode f = q.remove();

            //If the element is NULL, that means, we have completed a level. 
            if(f == null){
                //Add NULL to end of the queue so mark the next level because at this moment, we must have completed adding all childer of the current level
                q.add(null);
                //Add current level to answer
                res.add(temp);
                //Reset array so store next level
                temp = new ArrayList<>();
            }else{
                //Else, add the current element to current level
                temp.add(f.val);
                //Add childer to the queue
                if(f.left != null) q.add(f.left);
                if(f.right != null) q.add(f.right);
            }
        }
        
        //Add final level
        res.add(temp);
        
        return res;
        
    }
}
```
