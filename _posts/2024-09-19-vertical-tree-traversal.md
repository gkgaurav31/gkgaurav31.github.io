---
layout: post
title: Vertical Tree Traversal (geeksforgeeks - SDE Sheet)
date: 2024-09-19 22:23 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Binary Tree, find the vertical traversal of it starting from the leftmost level to the rightmost level.
If there are multiple nodes passing through a vertical line, then they should be printed as they appear in level order traversal of the tree.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/print-a-binary-tree-in-vertical-order/1?page=8)

## SOLUTION

We assign a column number to each node, starting with the root node at position 0 (considered as the middle or center column). Nodes to the left of the root will have negative column numbers (-1, -2, -3, etc.), while nodes to the right will have positive column numbers (1, 2, 3, etc.).

We traverse the tree using level-order traversal (BFS), processing one level at a time from left to right. We initialize the queue for level-order traversal with `(root, 0)`, where 0 represents the root's column number. When adding a left child to the queue, we add `(leftNode, parentNode's column - 1)`. Similarly, when adding a right child, we add `(rightNode, parentNode's column + 1)`.

For each node retrieved from the queue during traversal, we store it in a `Map<Column number, List<Node Values>>`.

```java
class Solution
{
    static ArrayList <Integer> verticalOrder(Node root)
    {

        ArrayList<Integer> verticalView = new ArrayList<>();

        if (root == null)
            return verticalView;

        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(root, 0));

        Map<Integer, List<Integer>> map = new TreeMap<>();

        while (!q.isEmpty()) {

            int size = q.size();

            for (int i = 0; i < size; i++) {

                Pair pair = q.poll();
                Node node = pair.node;
                int position = pair.position;

                if (!map.containsKey(position))
                    map.put(position, new ArrayList<Integer>());

                map.get(position).add(node.data);

                if (node.left != null)
                    q.add(new Pair(node.left, position - 1));

                if (node.right != null)
                    q.add(new Pair(node.right, position + 1));

            }

        }

        for (List<Integer> list : map.values()) {
            verticalView.addAll(list);
        }

        return verticalView;


    }
}

class Pair {

    Node node;
    int position;  // The horizontal position of the node

    Pair(Node n, int p) {
        node = n;
        position = p;
    }

}
```
