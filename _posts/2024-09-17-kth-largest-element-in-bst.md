---
layout: post
title: Kth largest element in BST (geeksforgeeks - SDE Sheet)
date: 2024-09-17 20:28 +0530
author: "Gaurav Kumar"
tags: "java binarytree BST geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Search Tree. Your task is to complete the function which will return the Kth largest element without doing any modification in Binary Search Tree.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/kth-largest-element-in-bst/1?page=8)

## SOLUTION

```java
class Solution
{

    int count = 0;
    int result = -1;

    public int kthLargest(Node root,int K)
    {

        kthLargestHelper(root, K);
        return result;

    }

    public void kthLargestHelper(Node root, int k){

        if(root == null || count >= k)
            return;

        kthLargestHelper(root.right, k);

        count++;

        if(count == k){
            result = root.data;
            return;
        }

        kthLargestHelper(root.left, k);

    }

}
```

### WITHOUT USING GLOBAL VARIABLE

Thanks to [Neha Bansal](https://www.linkedin.com/in/neha-bansal-48983280/) for helping with this code.

```java
class Solution
{
    public int kthLargest(Node root,int K)
    {
        return kthLargest(root, K, new Count(0));
    }

    public int kthLargest(Node root, int K, Count count){

        if(root == null)
            return -1;

        int right = kthLargest(root.right, K, count);
        if (right != -1) {
            return right;
        }

        count.c++;

        if(count.c == K)
            return root.data;

        return kthLargest(root.left, K, count);

    }

}

class Count{

    int c=0;

    Count(int c){
        this.c = c;
    }

}
```
