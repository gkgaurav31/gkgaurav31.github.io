---
layout: post
title: Graph is Tree or Not (geeksforgeeks - SDE Sheet)
date: 2024-09-20 20:35 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given an undirected graph of N nodes (numbered from 0 to N-1) and M edges. Return 1 if the graph is a tree, else return 0.

Note: The input graph can have self-loops and multiple edges.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/is-it-a-tree/1?page=8)

## SOLUTION

We can solve this by using a breadth-first search (BFS) approach to traverse the graph while keeping track of visited nodes. We create an adjacency list from the given edges and start the traversal from an arbitrary node (usually node 0). During the traversal, we check for cycles and self-loops, marking each visited node. Finally, we verify if all nodes were visited, which confirms whether the graph is a tree (connected and acyclic).

Major Steps:

- Create an adjacency list from the edges.
- Start BFS from an arbitrary node. We should be able to visit all nodes from any given node.
- Cycle detection: Check for cycles and self-loops during traversal.
- Visited tracking: Mark nodes as visited and count them.
- Connectivity check: Ensure all nodes are visited to confirm the graph is a tree.

```java
class Solution {

    public boolean isTree(int n, int m, ArrayList<ArrayList<Integer>> edges)
    {

        // Create adjacency list representation of the graph
        List<Integer>[] adj = createGraph(n, edges);

        // Array to keep track of visited nodes
        boolean[] visited = new boolean[n];

        // Count of visited nodes
        int visitedCount = 0;

        // Initialize a queue for BFS and start with node 0
        Queue<Integer> q = new LinkedList<>();
        q.add(0);

        // BFS traversal of the graph
        while(!q.isEmpty()){

            // Get the current node
            Integer current = q.poll();

            // If the current node is already visited, there is a cycle
            if(visited[current])
                return false;

            // Mark the current node as visited
            visited[current] = true;

            // Increment the visited nodes count
            visitedCount++;

            // Get all the neighbors of the current node
            List<Integer> neighbors = adj[current];

            // Process each neighbor
            for(int neighbor: neighbors){

                // Check for self-loop, which is not allowed in a tree
                if(current == neighbor)
                    return false;

                // If the neighbor has not been visited yet, add it to the queue
                if(!visited[neighbor])
                    q.add(neighbor);

            }

        }

        // Return true if all nodes were visited (graph is connected)
        return visitedCount == n;
    }

    public List<Integer>[] createGraph(int n, ArrayList<ArrayList<Integer>> edges){

        // Initialize adjacency list
        List<Integer>[] adj = new List[n];

        for(int i=0; i<n; i++)
            adj[i] = new ArrayList<>();

        // Fill the adjacency list with edges
        for(int i=0; i<edges.size(); i++){

            int u = edges.get(i).get(0); // Get node u
            int v = edges.get(i).get(1); // Get node v

            // Since the graph is undirected, add edges in both directions
            adj[u].add(v);
            adj[v].add(u);
        }

        return adj;
    }
}
```
