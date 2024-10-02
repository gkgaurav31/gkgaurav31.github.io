---
layout: post
title: DFS of Graph (geeksforgeeks - SDE Sheet)
date: 2024-10-02 19:25 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a connected undirected graph. Perform a Depth First Traversal of the graph.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1?page=9)

## SOLUTION

To implement DFS, we use a recursive helper function that explores each node and its neighbors in depth-first order. We maintain a visited array to track which nodes have already been visited. Starting from node 0, we mark it as visited, add it to the result list, and then recursively visit all its unvisited neighbors. This process continues until all connected nodes are visited, and the result list containing the DFS traversal order is returned.

```java
class Solution {

    public ArrayList<Integer> dfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {

        ArrayList<Integer> list = new ArrayList<>();
        boolean[] visited = new boolean[V];

        // Start DFS from node 0
        dfsHelper(0, adj, visited, list);

        return list;
    }

    public void dfsHelper(int node, ArrayList<ArrayList<Integer>> adj, boolean[] visited, ArrayList<Integer> list) {

        // If the node is already visited, return to avoid revisiting
        if (visited[node])
            return;

        // Mark the current node as visited
        visited[node] = true;

        // Add the current node to the result list
        list.add(node);

        // Get all neighbors (adjacent nodes) of the current node
        ArrayList<Integer> neighbors = adj.get(node);

        // Recursively visit all unvisited neighbors
        for (Integer neighbor: neighbors) {
            dfsHelper(neighbor, adj, visited, list);
        }
    }
}
```
