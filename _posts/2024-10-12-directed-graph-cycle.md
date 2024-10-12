---
layout: post
title: Directed Graph Cycle (geeksforgeeks - SDE Sheet)
date: 2024-10-12 18:02 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, check whether it contains any cycle or not.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1?page=9)

## SOLUTION

### APPROACH 1 - USING DFS (STACK OVERFLOW FOR LARGE #NODES)

```java
class Solution {

    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {

        boolean[] visited = new boolean[V];

        // track nodes in the current recursion path
        boolean[] path = new boolean[V];

        // Iterate over all vertices
        for (int i = 0; i < V; i++) {

            // If a cycle is found, return true
            if (isCyclicHelper(V, adj, visited, path, i)) {
                return true;
            }

        }

        // No cycle found in any DFS traversal, return false
        return false;

    }

    public boolean isCyclicHelper(int V, ArrayList<ArrayList<Integer>> adj, boolean[] visited, boolean[] path, int i) {

        // If the node is already in the current path, a cycle is detected
        if (path[i]) {
            return true;
        }

        // If the node is already visited, no need to explore again, return false
        if (visited[i]) {
            return false;
        }

        // Mark the current node as visited
        visited[i] = true;

        // Add the current node to the recursion path
        path[i] = true;

        // Check all adjacent vertices
        for (int neighbor : adj.get(i)) {

            // If any adjacent node causes a cycle, return true
            if (isCyclicHelper(V, adj, visited, path, neighbor)) {
                return true;
            }

        }

        // Remove the current node from the recursion path (backtrack)
        path[i] = false;

        // No cycle detected
        return false;

    }
}
```

### APPROACH 2 - USING INDEGREE (ACCEPTED)

```java
class Solution {

    // Kahn's Algorithm (BFS-based topological sorting)
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {

        //in-degrees for all vertices
        int[] indegree = new int[V];

        for (int i = 0; i < V; i++) {
            for (Integer j : adj.get(i)) {
                indegree[j]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();

        // add vertices with zero in-degree
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.add(i);
            }
        }

        // keep removing that node from the queue
        // assume is it removed from the graph
        // that means, indegree will reduce for all its adjacent nodes (neighbors). If it becomes 0, add that neighbor
        // the expectation is that all nodes should be added eventually if the graph has no cycle
        while (!q.isEmpty()) {

            // Dequeue a vertex
            int x = q.poll();

            // Get all neighbors of the dequeued vertex
            ArrayList<Integer> neighbors = adj.get(x);

            // Decrease the in-degree of all adjacent vertices
            for (Integer n : neighbors) {

                indegree[n]--;

                // If in-degree of a neighbor becomes zero, add it to the queue
                if (indegree[n] == 0) {
                    q.add(n);
                }

            }
        }

        // Check if there are any vertices with non-zero in-degree
        // If any vertex still has a non-zero in-degree, it means a cycle exists
        for (int i = 0; i < V; i++) {
            if (indegree[i] != 0)
                return true;
        }

        // If all vertices have zero in-degree, no cycle exists
        return false;
    }
}
```
