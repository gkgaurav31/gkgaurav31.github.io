---
layout: post
title: Undirected Graph Cycle (geeksforgeeks - SDE Sheet)
date: 2024-10-02 20:56 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an undirected graph with V vertices labelled from 0 to V-1 and E edges, check whether it contains any cycle or not. Graph is in the form of adjacency list where adj[i] contains all the nodes ith node is having edge with.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1?page=9)

## SOLUTION

[Good Explanation - geeksforgeeks](https://www.youtube.com/watch?v=6ZRhq2oFCuo)

Let's say we start from node 0, and node 1 is its neighbor. So, from node 0, we go to node 1. If we only track whether a node has been visited, this can cause a problem because node 0 will again appear as a neighbor of node 1, since the graph is undirected.

To handle this, we need to track the parent node. If a neighbor is not the parent, then we can conclude that a cycle has been found.

For every visited vertex `v`, if there is an adjacent `u` such that `u` is already visited and `u` is not parent of `v`, then there is a cycle in graph.

```java
class Solution {

    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {

        boolean[] visited = new boolean[V];

        // Traverse each node in the graph
        // This is needed since the graph can be disconnected
        for(int node = 0; node < V; node++) {

            // If the node is not yet visited, initiate DFS from this node
            if(!visited[node]) {

                // If DFS finds a cycle, return true
                if(dfs(adj, visited, node, -1))
                    return true;
            }
        }

        // no cycle found
        return false;
    }

    public boolean dfs(ArrayList<ArrayList<Integer>> adj, boolean[] visited, int node, int parent) {

        // Mark the current node as visited
        visited[node] = true;

        // Explore all adjacent nodes (neighbors)
        for(Integer neighbor : adj.get(node)) {

            // If the neighbor hasn't been visited, perform DFS on the neighbor
            if(!visited[neighbor]) {

                // If DFS finds a cycle, return true
                if(dfs(adj, visited, neighbor, node))
                    return true;

            } else {

                // If the neighbor is visited and is not the parent node, it's a cycle
                if(neighbor != parent)
                    return true;
            }
        }

        return false;
    }
}
```
