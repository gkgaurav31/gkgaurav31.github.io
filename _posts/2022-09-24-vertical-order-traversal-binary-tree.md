---
layout: post
title: Vertical Order Traversal - Binary Tree
date: 2022-09-24 23:58 +0530
author: "Gaurav Kumar"
tags: "java binarytree important"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given a binary tree, return a 2-D array with vertical order traversal of it.

### SOLUTION

```java

public class Solution {
    
    public ArrayList<ArrayList<Integer>> verticalOrderTraversal(TreeNode A) {

        TreeNode root = A;

        if(root == null) return new ArrayList<ArrayList<Integer>>();
        
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root, 0));
        
        Map<Integer, ArrayList<Integer>> map = new HashMap<>();
        
        int minLevel=0;
        int maxLevel=0;
        
        while(!q.isEmpty()){
            
            Pair p = q.remove();
            
            TreeNode node = p.node;
            int level = p.level;
            
            if(!map.containsKey(level)){
                ArrayList<Integer> list = new ArrayList<Integer>();
                list.add(node.val);
                map.put(level, list);
            }else{
                map.get(level).add(node.val);
            }
            
            if(node.left != null){
                q.add(new Pair(node.left, level-1));    
                minLevel = Math.min(minLevel, level-1);
            }
            if(node.right != null){
                q.add(new Pair(node.right, level+1));    
                maxLevel = Math.max(maxLevel, level+1);
            }
            
        }
            ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
            
            for(int i=minLevel; i<=maxLevel; i++){
                res.add(map.get(i));
            }
            
            return res;
        
    }

}

class Pair{
    TreeNode node;
    int level;
    public Pair(TreeNode node, int level){
        this.node = node;
        this.level = level;
    }
}

```
