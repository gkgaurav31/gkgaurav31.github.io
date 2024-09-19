---
layout: post
title: Bottom View of Binary Tree
date: 2024-09-19 21:57 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree, return an array where elements represent the bottom view of the binary tree from left to right.

Note: If there are multiple bottom-most nodes for a horizontal distance from the root, then the latter one in the level traversal is considered.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1?page=8)

## SOLUTION

```java
class Solution {

    public ArrayList<Integer> bottomView(Node root) {

        ArrayList<Integer> bottomView = new ArrayList<>();

        // Base case: if the tree is empty, return an empty list
        if (root == null)
            return bottomView;

        // Queue to perform a level-order traversal, storing nodes along with their horizontal positions
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root, 0));

        // The root node will be considered to be at position 0. This of it as a column number. At nodes left to it will have column numbers like -1, -2, -3 etc. and the nodes to the right side of it will have the column number as 1, 2, 3 etc.
        // We will store nodes for each column in this Map
        // We want this map to be sorted based on the column number, so we are using a TreeMap
        Map<Integer, List<Integer>> map = new TreeMap<>();

        // Perform level-order traversal (BFS) on the tree
        while (!q.isEmpty()) {

            int size = q.size();

            // Traverse each node at the current level
            for (int i = 0; i < size; i++) {

                // Get the next node and its horizontal position from the queue
                Pair pair = q.poll();
                Node node = pair.node;
                int position = pair.position;

                // If the horizontal position is not yet in the map, add it
                if (!map.containsKey(position))
                    map.put(position, new ArrayList<Integer>());

                // Add the node's value to the list for this horizontal position
                // As per the question, if two nodes have the same column number, the right should be taken
                // We are adding the right node later, so it will appear later in the final column list in the Map
                map.get(position).add(node.data);

                // If the node has a left child, add it to the queue with a horizontal position one less than the current node
                if (node.left != null)
                    q.add(new Pair(node.left, position - 1));

                // If the node has a right child, add it to the queue with a horizontal position one more than the current node
                if (node.right != null)
                    q.add(new Pair(node.right, position + 1));

            }

        }

        // After traversing the tree, add the last element (the bottom-most node) from each horizontal position to the result
        for (List<Integer> list : map.values()) {
            bottomView.add(list.get(list.size() - 1));
        }

        return bottomView;

    }
}

// Class to store a node and its horizontal position in the tree
class Pair {

    Node node;
    int position;  // The horizontal position of the node

    Pair(Node n, int p) {
        node = n;
        position = p;
    }

}
```
