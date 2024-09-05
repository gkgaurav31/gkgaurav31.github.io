---
layout: post
title: Level order traversal in spiral form (geeksforgeeks - SDE Sheet)
date: 2024-09-05 21:10 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary tree and the task is to find the spiral order traversal of the tree.

Spiral order Traversal mean: Starting from level 0 for root node, for all the even levels we print the node's value from right to left and for all the odd levels we print the node's value from left to right.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/level-order-traversal-in-spiral-form/1?page=5)

## SOLUTION

We can use level order traversal. We can maintain the current level in a variable, incremented with 1 after each iteration. When the level is even, we can reverse the node list.

```java
class Spiral
{
    ArrayList<Integer> findSpiral(Node root)
    {

        // queue for level order traversal
        Queue<Node> q = new LinkedList<>();

        // add root to the queue
        q.add(root);

        // result
        ArrayList<Integer> list = new ArrayList<>();

        // track current level
        int level = 0;

        // while queue is not empty
        while(!q.isEmpty()){

            // get num of nodes in the current level
            int size = q.size();

            // nodes in the current level
            ArrayList<Integer> currentLevel = new ArrayList<>();

            // add them one by one to the current level list
            // add their left and right to the queue to visit them for the next level
            for(int i=0; i<size; i++){

                Node currentNode = q.poll();

                currentLevel.add(currentNode.data);

                if(currentNode.left != null)
                    q.add(currentNode.left);

                if(currentNode.right != null)
                    q.add(currentNode.right);

            }

            // even levels we print the node's value from right to left
            if(level%2 == 0)
                Collections.reverse(currentLevel);

            // add the current level to the overall result
            list.addAll(currentLevel);

            // increase level for the next iteration
            level++;

        }

        return list;

    }

}
```
