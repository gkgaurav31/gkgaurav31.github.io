---
layout: post
title: Left View of Binary Tree (geeksforgeeks - SDE Sheet)
date: 2024-09-17 15:39 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Tree, return its Left view. The left view of a Binary Tree is a set of nodes visible when the tree is visited from the Left side. If no left view is possible, return an empty array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/left-view-of-binary-tree/1?page=7)

## SOLUTION

We can solve this by performing a level-order traversal of the binary tree using a queue. As we process each level, we collect all nodes at that level and then add the first node of each level to a result list, which represents the left view of the tree.

```java
class Tree
{

    ArrayList<Integer> leftView(Node root)
    {
        // List to hold nodes at each level of the tree.
        List<List<Node>> list = new ArrayList<>();

        // Queue to perform level-order traversal of the tree.
        Queue<Node> q = new LinkedList<>();
        q.add(root); // Start with the root node.

        // Process the tree level by level.
        while(!q.isEmpty()){

            // List to hold nodes of the current level.
            List<Node> level = new ArrayList<>();

            // Number of nodes at the current level.
            int size = q.size();

            // Traverse nodes at the current level.
            for(int i = 0; i < size; i++){

                // Remove node from the queue.
                Node current = q.poll();

                // Add node to the current level list.
                level.add(current);

                // Add left child to the queue if it exists.
                if(current.left != null)
                    q.add(current.left);

                // Add right child to the queue if it exists.
                if(current.right != null)
                    q.add(current.right);

            }

            // Add the list of nodes at the current level to the overall list.
            list.add(level);
        }

        // List to store the left view of the tree.
        ArrayList<Integer> leftView = new ArrayList<>();

        // For each level, add the first node (leftmost) to the left view list.
        for(int i = 0; i < list.size(); i++){
            leftView.add(list.get(i).get(0).data);
        }

        return leftView;
    }
}
```

## OPTIMIZATION

Instead of adding all the nodes to our result, we will check if the size of the leftView list is < currentLevel, which means we have not added any node for the currentLevel. The first node which needs to be added at each level will form the leftView of the tree.

```java
class Tree
{

    ArrayList<Integer> leftView(Node root)
    {

        ArrayList<Integer> leftView = new ArrayList<>();

        Queue<Node> q = new LinkedList<>();

        q.add(root);

        int currentLevel = 0;

        while(!q.isEmpty()){

            currentLevel++;

            int size = q.size();

            for(int i=0; i<size; i++){

                Node current = q.poll();

                if(leftView.size() < currentLevel)
                    leftView.add(current.data);

                if(current.left != null)
                    q.add(current.left);

                if(current.right != null)
                    q.add(current.right);

            }

        }

        return leftView;

    }

}
```
