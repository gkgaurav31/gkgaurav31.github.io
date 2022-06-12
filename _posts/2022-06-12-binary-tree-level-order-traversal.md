---
layout: post
title: Binary Tree Level Order Traversal
date: 2022-06-12 22:59 +0530
tags: "java BST"
categories: "leetcode"
---

## Problem Description:  [Binary Tree Level Order Traversal](https://leetcode.com/explore/learn/card/data-structure-tree/134/traverse-a-tree/931/)

### Solution

For this, we can use a Queue Data Structure. We first keep the root element already pushed in the queue. Then we dequeue "all elements in the queue" by taking the size of the queue. (For the first time, it will iterate just once since we would have just the root element). While dequeuing the elements, check if that Node has any left/right child. If it does have it, then push it to the queue. Make sure to loop through as per the queue size, and not just based on if queue is empty because we would keep adding more elements to the queue as we move forward.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        
        List<List<Integer>> output = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        
        if(root == null) return new ArrayList<List<Integer>>();
        
        q.offer(root);
        
        while(!q.isEmpty()){
            
            int size = q.size();
            List<Integer> tempList = new ArrayList<Integer>();
            
            for(int i=0; i<size; i++){
                
                TreeNode c = q.poll();
                tempList.add(c.val);
                
                if(c.left != null){
                    q.offer(c.left);
                }
                
                if(c.right != null){
                    q.offer(c.right);
                }
                
            }
            
            output.add(tempList);
            
        }
        
        return output;
        
    }
}
```
