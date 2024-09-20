---
layout: post
title: Merge two BST's (geeksforgeeks - SDE Sheet)
date: 2024-09-20 20:26 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two BSTs, return elements of merged BSTs in sorted form.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/merge-two-bst-s/1?page=8)

## SOLUTION

We can solve this by performing inorder traversals on both binary search trees to get sorted lists, then merging them using a two-pointer technique to create a single sorted list efficiently.

```java
class Solution {

    public List<Integer> merge(Node root1, Node root2) {
        // Perform inorder traversal on both trees and merge the sorted lists
        return mergerSortedLists(inorder(root1, new ArrayList<>()), inorder(root2, new ArrayList<>()));
    }

    // Merges two sorted lists into one sorted list
    public List<Integer> mergerSortedLists(List<Integer> list1, List<Integer> list2){

        int i = 0;
        int j = 0;

        List<Integer> list = new ArrayList<>();

        while(i < list1.size() && j < list2.size()){
            if(list1.get(i) < list2.get(j)){
                list.add(list1.get(i));
                i++;
            } else {
                list.add(list2.get(j));
                j++;
            }
        }

        while(i < list1.size()){
            list.add(list1.get(i));
            i++;
        }

        while(j < list2.size()){
            list.add(list2.get(j));
            j++;
        }

        return list;
    }

    public List<Integer> inorder(Node root, List<Integer> list){

        if(root == null)
            return list;

        inorder(root.left, list);
        list.add(root.data);
        inorder(root.right, list);

        return list;
    }
}
```
