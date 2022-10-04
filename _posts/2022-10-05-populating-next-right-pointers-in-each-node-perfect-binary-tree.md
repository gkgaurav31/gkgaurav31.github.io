---
layout: post
title: Populating Next Right Pointers in Each Node - Perfect Binary Tree
date: 2022-10-05 00:44 +0530
tags: "java binarytree"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.
Initially, all next pointers are set to NULL.  
[leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

### SOLUTION

```java
class Solution {
    
    public Node connect(Node root) {
        
        //Get level order traversal of the BT
        List<List<Node>> levels = levelOrder(root);

        //Iterate through each level
        for(int i=0; i<levels.size(); i++){

            //Get the level i
            List<Node> level = levels.get(i);

            //For each Node, set the next pointer to its next
            for(int j=0; j<level.size()-1; j++){
            level.get(j).next = level.get(j+1);
            }

            //We don't really need to update the next for the last node since it would already be null
            //level.get(level.size()-1).next = null;

        }
        return root;
    }
    
    //Get the Level Order Traversal on the BT
    public List<List<Node>> levelOrder(Node root) {
        
        //If the BST is null
        if(root == null) return new ArrayList<List<Node>>();
        
        //We will make the Queue type as TreeNode because we need to access the left and right children later
        Queue<Node> q = new LinkedList<>();

        //Initialize the queue with the root node and NULL
        //The idea is to add NULL after every level
        //While removing elements from the queue, if we encounter a NULL, that means that we have covered a certain level
        //So we can append the current list to the answer and continue
        q.add(root);
        q.add(null);
        
        //To store final answer
        List<List<Node>> res = new ArrayList<>();

        //To store each level
        List<Node> temp = new ArrayList<>();
        
        //If there is one node and NULL, size must be at least 1
        while(q.size() > 1){

            //Remove the element from the queue
            Node f = q.remove();

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
                temp.add(f);
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
