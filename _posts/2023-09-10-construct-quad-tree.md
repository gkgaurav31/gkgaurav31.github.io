---
layout: post
title: Construct Quad Tree
date: 2023-09-10 11:11 +0530
author: "Gaurav Kumar"
tags: "java divide_and_conquer leetcode leetcode150"
categories: "divide_and_conquer"
---

## PROBLEM DESCRIPTION

Given a n \* n matrix grid of 0's and 1's only. We want to represent grid with a Quad-Tree.
Return the root of the Quad-Tree representing grid.

```text
A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

    val: True if the node represents a grid of 1's or False if the node represents a grid of 0's. Notice that you can assign the val to True or False when isLeaf is False, and both are accepted in the answer.
    isLeaf: True if the node is a leaf node on the tree or False if the node has four children.

class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}

We can construct a Quad-Tree from a two-dimensional area using the following steps:

    If the current grid has the same value (i.e all 1's or all 0's) set isLeaf True and set val to the value of the grid and set the four children to Null and stop.
    If the current grid has different values, set isLeaf to False and set val to any value and divide the current grid into four sub-grids as shown in the photo.
    Recurse for each of the children with the proper sub-grid.
```

[leetcode](https://leetcode.com/problems/construct-quad-tree/)

## SOLUTION

Good Explanation: [Neetcode YouTube](https://www.youtube.com/watch?v=UQ-1sBMV0v4)

```java
// Definition for a QuadTree node.
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    // Constructor for initializing a leaf node
    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    // Constructor for initializing a leaf node with a given value
    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    // Constructor for initializing a non-leaf node with children
    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
};

class Solution {

    // Main method to construct the QuadTree
    public Node construct(int[][] grid) {
        return helper(grid, grid.length, 0, 0); //the given grid is a square, so we just need the grid length. we will start from first cell [0,0]
    }

    // Recursive helper method to build the QuadTree
    public Node helper(int[][] grid, int n, int r, int c) {

        // If the current grid has the same value for all elements, create a leaf node
        if (hasAllSame(grid, n, r, c)) {
            return new Node(grid[r][c] == 1, true); //the val of the Node should be true if all cells are 1 or false if all cells are 0
        }

        int half = n / 2; // Calculate the size of the sub-grid

        // Recursively build each quadrant of the QuadTree
        Node topLeft = helper(grid, half, r, c);
        Node topRight = helper(grid, half, r, c + half);
        Node bottomLeft = helper(grid, half, r + half, c);
        Node bottomRight = helper(grid, half, r + half, c + half);

        // Create a non-leaf node with the four quadrants as children
        return new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);
    }

    // Helper function to check if all elements in a sub-grid have the same value
    public boolean hasAllSame(int[][] grid, int n, int r, int c) {
        int val = grid[r][c];

        for (int i = 0; i < n; i++) { //IMPORTANT: here we are iterating from [0,n-1] and later we are adding r or c to get the actual co-ordinate in the subarray
            for (int j = 0; j < n; j++) {
                if (val != grid[r + i][c + j])
                    return false;
            }
        }

        return true;
    }
}
```
