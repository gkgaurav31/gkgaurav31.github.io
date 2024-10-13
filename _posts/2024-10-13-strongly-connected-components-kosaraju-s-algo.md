---
layout: post
title: Strongly Connected Components (Kosaraju's Algo) (geeksforgeeks - SDE Sheet)
date: 2024-10-13 15:54 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, Find the number of strongly connected components in the graph.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/strongly-connected-components-kosarajus-algo/1?page=9)

## SOLUTION

[Good Explanation - takeUForward](https://www.youtube.com/watch?v=R6uoSjZ2imo)

- We want to find the strongly connected components (SCCs) in a directed graph, where each SCC is a group of nodes where every node can reach every other node.
- First, we perform a DFS on the graph and store the nodes in a stack based on their finish time (when all neighbors are visited).
- Then, we reverse the graph by reversing the direction of all edges.
- After that, we perform DFS again on the reversed graph, using the nodes in the order stored in the stack.
- Each DFS on the reversed graph identifies a new strongly connected component (SCC).

```java
class Solution {

    public int kosaraju(int V, ArrayList<ArrayList<Integer>> adj) {

        // Stack to store vertices based on their finishing time in the first DFS
        Stack<Integer> stack = new Stack<>();

        boolean[] visited = new boolean[V];

        // Perform DFS on each vertex
        for (int i = 0; i < V; i++) {
            if (!visited[i])
                dfs1(V, adj, stack, i, visited);
        }

        // Reverse the edges of the graph (transpose of the graph)
        ArrayList<ArrayList<Integer>> adjReversed = new ArrayList<>();

        for (int i = 0; i < V; i++)
            adjReversed.add(new ArrayList<Integer>());

        // Reverse all edges in the adjacency list
        for (int i = 0; i < V; i++) {
            for (Integer j : adj.get(i)) {
                // Reverse edge direction
                adjReversed.get(j).add(i);
            }
        }

        // Initialize visited array again for the second DFS on the reversed graph
        int count = 0;
        visited = new boolean[V];

        // Process all nodes in the order of their finish time (from the stack)
        while (!stack.isEmpty()) {

            // Get the next node based on finish time
            int node = stack.pop();

            // Perform DFS on the reversed graph if the node is not yet visited
            if (!visited[node]) {

                // Each DFS call in the reversed graph represents a new SCC
                count++;

                dfs2(V, adjReversed, node, visited);

            }
        }

        return count;
    }

    // First DFS function to record the finishing order of nodes
    public void dfs1(int V, ArrayList<ArrayList<Integer>> adj, Stack<Integer> stack, int node, boolean[] visited) {

        // Base case: return if the node is already visited
        if (visited[node])
            return;

        // Mark the current node as visited
        visited[node] = true;

        // Recursively call DFS for all adjacent nodes
        for (Integer n : adj.get(node)) {
            dfs1(V, adj, stack, n, visited);
        }

        // After visiting all neighbors, push the current node onto the stack
        stack.push(node);
    }

    // Second DFS function on the reversed graph to identify SCCs
    public void dfs2(int V, ArrayList<ArrayList<Integer>> adj, int node, boolean[] visited) {

        // Base case: return if the node is already visited
        if (visited[node])
            return;

        // Mark the current node as visited
        visited[node] = true;

        // Recursively call DFS for all adjacent nodes in the reversed graph
        for (Integer n : adj.get(node)) {
            dfs2(V, adj, n, visited);
        }
    }
}
```
