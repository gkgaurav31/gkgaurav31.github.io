---
layout: post
title: Equal Tree Partition - Binary Tree
date: 2022-10-01 23:14 +0530
author: "Gaurav Kumar"
tags: "java binarytree important leetcode"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

Given the root of a binary tree, return true if you can partition the tree into two trees with equal sums of values after removing exactly one edge on the original tree.
[leetcode](https://leetcode.com/problems/equal-tree-partition/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/equal_binary_tree_partition.png)

The main idea is that sub of the two subtrees after breaking the edge should have half the sum of the complete main tree. So, in the first iteration, we get the complete sum of the given binary tree. Then, in the next iteration, we check if there is any subtree which has half of that sum.  

There is a very important edge case here, as shown in the above diagram. If the sum of complete tree is 0, we need to make sure that we are checking sum of sumtrees only in the "check" method. One way to do this is by passing the main root node as a parameter and check whether it's not equal.

```java
class Solution {
    
    int count = 0;
    
    public boolean checkEqualTree(TreeNode root) {

        long sum = getSum(root);
        
        if(sum%2 != 0) return false;

        return check(root, sum/2, root);
    }
    
    public boolean check(TreeNode root, long sum, TreeNode mainRoot){

        if(root == null) return false;

        long s = root.val + getSum(root.left) + getSum(root.right);
        if(s == sum && root != mainRoot) return true;
        else{
            return check(root.left, sum, mainRoot) || check(root.right, sum, mainRoot);
        }
    }

    public long getSum(TreeNode A){
        if(A == null) return 0;
        return A.val + getSum(A.left) + getSum(A.right);
    }
}
```
