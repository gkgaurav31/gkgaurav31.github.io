---
layout: post
title: Connect Nodes at Same Level (geeksforgeeks - SDE Sheet)
date: 2024-09-19 22:34 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree, connect the nodes that are at same level. You'll be given an addition nextRight pointer for the same.

Initially, all the nextRight pointers point to garbage values. Your function should set these pointers to point next right for each node.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/connect-nodes-at-same-level/1?page=8)

## SOLUTION

We can solve this by using a level order traversal with a queue to connect all nodes at the same level. We start by adding the root to the queue and then process each level iteratively. For each level, we use a prev pointer to connect nodes by setting their nextRight pointers. We enqueue the left and right children of each node as we go. This ensures that all nodes at a level are connected to their immediate right neighbor.

```java
class Solution
{

    public void connect(Node root)
    {
        // Base case: if the root is null, there is nothing to connect
        if(root == null)
            return;

        // Create a queue to perform level order traversal
        Queue<Node> q = new LinkedList<>();

        // init: add the root node to the queue
        q.add(root);

        // Process nodes level by level
        while(!q.isEmpty()){

            // Get the number of nodes at the current level
            int size = q.size();

            // Pointer to keep track of the previous node at the current level
            Node prev = null;

            // Iterate through all nodes at the current level
            for(int i=0; i<size; i++){

                // Remove the front node from the queue
                Node current = q.poll();

                // If this is the first node at this level, just set 'prev' to 'current'
                if(prev == null){

                    prev = current;

                } else {

                    // Otherwise, set the 'nextRight' pointer of the previous node to 'current'
                    prev.nextRight = current;

                    // Update 'prev' to be 'current'
                    prev = current;

                }

                // Add the left child of the current node to the queue, if it exists
                if(current.left != null)
                    q.add(current.left);

                // Add the right child of the current node to the queue, if it exists
                if(current.right != null)
                    q.add(current.right);

            }

        }
    }
}
```
