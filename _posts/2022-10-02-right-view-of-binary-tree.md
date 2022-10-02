---
layout: post
title: Right View of Binary tree
date: 2022-10-02 20:44 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.[Leetcode](https://leetcode.com/problems/binary-tree-right-side-view/)

### SOLUTION

The idea is to get the last/right-most element for each level in Level Order Traversal.

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<List<Integer>> list = levelOrder(root);

        ArrayList<Integer> res = new ArrayList<>();

        for(int i=0; i<list.size(); i++){
            res.add(list.get(i).get(list.get(i).size()-1));
        }

        return res;
    }
    

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
