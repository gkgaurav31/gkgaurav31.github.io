---
layout: post
title: Populating Next Right Pointers in Each Node II
date: 2023-08-11 20:02 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcode150"
categories: "binarytree"
---
## PROBLEM DESCRIPTION

Given a binary tree

```text
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

[leetcode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## SOLUTION

### APPROACH 1 (EXTRA SPACE)

The idea is to perform a level order traversal of the binary tree (skip the null nodes). Then iterate through nodes at each level and update their next pointers.

```java
class Solution {
    
    public Node connect(Node root) {

        // If the root is null, there's nothing to connect, so return null.
        if (root == null) return null;

        // Create a queue to perform level-order traversal.
        Queue<Node> q = new LinkedList<>();

        // Create a list to store nodes at each level.
        List<List<Node>> list = new ArrayList<>();

        // Add the root node to the queue to start traversal.
        q.add(root);
        // Add a marker (null) to indicate the end of the current level.
        q.add(null);

        // Continue traversal until there's only the null marker left in the queue.
        while (q.size() > 1) {

            // Create a list to store nodes at the current level.
            List<Node> currentList = new ArrayList<>();

            // Process nodes at the current level until the null marker is encountered.
            while (q.peek() != null) {

                // Remove a node from the queue.
                Node node = q.poll();

                // Add the node to the list of the current level.
                currentList.add(node);

                // Add the left and right child nodes of the current node to the queue for the next level.
                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }

            // Remove the null marker from the queue to move to the next level.
            // Add a new null marker to the queue for the next level.
            q.poll(); q.add(null);

            // If there are nodes at the current level, add the list to the overall list of levels.
            if (currentList.size() > 0) list.add(currentList);
        }

        // Traverse through the list of levels.
        for (int i = 0; i < list.size(); i++) {

            // Get the current node
            List<Node> l = list.get(i);

            // Traverse from first to last-but-one node because we do not need to update the next pointer for the last node
            // Update the next pointer of the current node to point to next node in that level
            for (int j = 0; j < l.size() - 1; j++) {
                l.get(j).next = l.get(j + 1);
            }
        }

        // Return the modified root of the binary tree.
        return root;
    }
}
```
