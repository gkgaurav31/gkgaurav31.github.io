---
layout: post
title: Recover Binary Search Tree
date: 2022-06-20 00:57 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "interviewbit"
---

### PROBLEM DESCRIPTION

Two elements of a binary search tree (BST) are swapped by mistake. Return a sorted list containing those values.  [Recover Binary Search Tree](https://www.interviewbit.com/problems/recover-binary-search-tree/)

### SOLUTION

We can do a inorder travel of the given tree. The inorder is sorted, so we can create another list which has the sorted values. After comparing the two lists, we can get the 2 elements which are not in the correct location.  
This approach takes O(n) time complexity and O(n) space complexity. To reduce the space complexity to O(1), we can use Morris Algorithm to get the inorder of the binary tree. See: [Morris Traversal: Inorder Tree Traversal without recursion and without stack!](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion-and-without-stack/)

```java
public class Solution {
    
    List<Integer> list = new ArrayList<>();
    
    public void inorder(TreeNode A){
        if(A == null) return;
        inorder(A.left);
        list.add(A.val);
        inorder(A.right);
    }
    
    public ArrayList<Integer> recoverTree(TreeNode A) {
        
        inorder(A);
        
        List<Integer> sorted = new ArrayList<>(list);
        Collections.sort(sorted);
        
        ArrayList<Integer> output = new ArrayList<Integer>();
        
        for(int i=0; i<list.size(); i++){
            if(list.get(i) != sorted.get(i)){
                output.add(list.get(i));
                output.add(sorted.get(i));
                Collections.sort(output);
                return output;
            }
        }
        
        return output;
        
    }
}
```
