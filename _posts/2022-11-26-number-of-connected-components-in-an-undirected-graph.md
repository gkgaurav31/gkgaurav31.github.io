---
layout: post
title: Number of Connected Components in an Undirected Graph
date: 2022-11-26 00:21 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs"
categories: "graphs"
---

### Problem Description

You have a graph of n nodes. You are given an integer n and an array edges where edges[i] = [ai, bi] indicates that there is an edge between ai and bi in the graph.

Return the number of connected components in the graph.

### Solution

The idea is that if we apply DFS/BFS to any node in certain connected component, then all connected nodes will be visited. So means that to cover all the Nodes, we will have to apply DFS/BFS multiple times and that will be equal to the number of connected components in the graph. So, to solve this, we can iterate through all the nodes one by one and apply DFS/BFS whenever we get a non-visited node. Each time when we apply BFS/DFS, we increase the countOfConnectedComponents by 1. If the node is already visited (from previous BFS/DFS which you would have done) then simply skip it.

```java
class Solution {

    public ArrayList<Integer>[] createGraph(int n, int[][] edges){

        ArrayList<Integer>[] graph = new ArrayList[n];

        for(int i=0; i<n; i++){
            graph[i] = new ArrayList<>();
        }

        for(int i=0; i<edges.length; i++){
            
            int a = edges[i][0];
            int b = edges[i][1];

            graph[a].add(b);
            graph[b].add(a);

        }

        return graph;

    }

    public void dfs(int s, boolean[] visited, ArrayList<Integer>[] adj){

        if(visited[s] == true) return;

        visited[s] = true;
        ArrayList<Integer> neighbours = adj[s];

        for(int i=0; i<neighbours.size(); i++){
            dfs(neighbours.get(i), visited, adj);
        }

    }

    //Get connected components given the number of Nodes and edges
    public int countComponents(int n, int[][] edges) {


        //There are N nodes given from 0 to N-1
        boolean[] visited = new boolean[n];

        //Create adjacency list from the given edges
        ArrayList<Integer>[] adj = createGraph(n, edges);

        //Count variable to track number of connected components
        int count = 0;

        //For each node, check if it's visited
        //If it's unvisited, apply DFS
        //Increase count when DFS is applied
        for(int i=0; i<n; i++){
            if(visited[i] == false){
                dfs(i, visited, adj);
                count++;
            }
        }

        return count;

    }

}
```
