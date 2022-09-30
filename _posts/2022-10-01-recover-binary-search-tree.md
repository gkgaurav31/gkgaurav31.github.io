---
layout: post
title: Recover Binary Search Tree
date: 2022-10-01 01:27 +0530
author: "Gaurav Kumar"
tags: "java BST interviewbit important"
categories: "BST"
---

### PROBLEM DESCRIPTION

Two elements of a binary search tree (BST), represented by root A are swapped by mistake. Tell us the 2 values swapping which the tree will be restored.  
[interviewbit](https://www.interviewbit.com/problems/recover-binary-search-tree/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/recover_bst.png)

```java
class Pair{
    int x;
    int y;
    Pair(int x, int y){
        this.x = x;
        this.y = y;
    }
} 

public class Solution {

    public ArrayList<Integer> recoverTree(TreeNode A) {

        ArrayList<Pair> list = new ArrayList<>();

        ArrayList<Integer> inorder = new ArrayList<>();
        inorder(A, inorder);

        if(inorder.size() <= 1) return null;

        for(int i=1; i<inorder.size(); i++){
            if(inorder.get(i) < inorder.get(i-1)){
                Pair p = new Pair(inorder.get(i-1), inorder.get(i));
                list.add(p);
            }
        }

        ArrayList<Integer> ans = new ArrayList<>();

        if(list.size() == 1){
            ans.add(list.get(0).y);
            ans.add(list.get(0).x);
            return ans;
        }

        ans.add(list.get(1).y);
        ans.add(list.get(0).x);

        return ans;

    }

    public void inorder(TreeNode root, List<Integer> list){
        if(root == null) return;
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```
