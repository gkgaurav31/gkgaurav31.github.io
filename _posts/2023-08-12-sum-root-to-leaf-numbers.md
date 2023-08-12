---
layout: post
title: Sum Root to Leaf Numbers
date: 2023-08-12 14:20 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcode150"
categories: "binarytree"
---
## PROBLEM DESCRIPTION

You are given the root of a binary tree containing digits from 0 to 9 only.
Each root-to-leaf path in the tree represents a number.
Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

[leetcode](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

## SOLUTION

```java
class Solution {
    
    public int sumNumbers(TreeNode root) {
        return traverse(root, new StringBuffer(), 0);
    }

    public int traverse(TreeNode root, StringBuffer sb, int total){

        //if node is null, simply return total
        if(root == null) return total;

        //if node is not null, append root.val to stringbuffer sb with the next digit which would form the number
        sb.append(root.val);

        //if current node is a leaf node
        //add the integer value of stringbuffer sb to total
        if(root.left == null && root.right == null){
            total += Integer.parseInt(sb.toString());
            return total;
        }

        //store the current stringbuffer which was added
        //while traversing the left sub tree, stringbuffer sb will be modified
        //but we need to use the current state of sb while traversing right sub tree also
        //this is like backtracking
        StringBuffer tempsb = new StringBuffer(sb);

        //recursively get the total sum for LST and RST
        int left = traverse(root.left, sb, total);
        int right = traverse(root.right, tempsb, total);

        //return the total from both LST and RST
        return left + right; 

    }

}
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
```

### ANOTHER WAY TO CODE

```java
class Solution {
    
    public int sumNumbers(TreeNode root) {
        return traverse(root, 0, 0); // Start the traversal with initial values
    }

    // Helper function for recursive traversal
    public int traverse(TreeNode root, int current, int total) {

        // If the current node is null, return the accumulated total
        if (root == null) return total; 
        
        // Build the current number by appending the current node's value
        current = current * 10 + root.val; 
        
        // If the current node is a leaf node, add the current number to the total
        if (root.left == null && root.right == null) {
            total += current;
            return total;
        }

        // Recursively traverse the left and right subtrees, passing along the updated current number and total
        int left = traverse(root.left, current, total);
        int right = traverse(root.right, current, total);

        return left + right; // Return the sum of values obtained from left and right subtrees
    }
}
```
