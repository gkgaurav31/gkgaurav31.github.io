---
layout: post
title: Number of Provinces
date: 2023-02-22 23:26 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

Given an undirected graph with V vertices. We say two vertices u and v belong to a single province if there is a path from u to v or v to u. Your task is to find the number of provinces.

Note: A province is a group of directly or indirectly connected cities and no other cities outside of the group.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/number-of-provinces/1)

### Solution

We need to basically find the number of connected components in the given graph. This is similar to this post:

[Number of Connected Components in an Undirected Graph]({% post_url 2022-11-26-number-of-connected-components-in-an-undirected-graph %})

```java
class Solution {
    
    static int numProvinces(ArrayList<ArrayList<Integer>> adj, int V) {

        //track visited nodes    
        boolean[] visited = new boolean[V];
        
        //number of connected components
        int count = 0;
        
        //try visited each node and apply DFS if it's not visited
        //after DFS (or BFS), all the nodes which are reachable will be marked as visited
        //keep iterating to other nodes, if those are not visited, that must be a new connected component (another province)
        for(int i=0; i<V; i++){
            if(visited[i] == false){
                dfs(i, adj, visited);
                count++;
            }            
        }
        
        return count;
    }
    
    static void dfs(Integer node, ArrayList<ArrayList<Integer>> adj, boolean[] visited){

        //if the node is already visited, return        
        if(visited[node]) return;
        
        //else mark it as visited
        visited[node] = true;
        
        //check for neighbours
        ArrayList<Integer> neighbours = adj.get(node);
        
        //as per the question, if adj.get(node1).get(node2) == 1, then they are connected
        for(int i=0; i<neighbours.size(); i++){
            //if current node connectes to node i, call DFS for node i recursively
            if(neighbours.get(i) == 1){
                dfs(i, adj, visited);
            }
        }
        
    }
};
```
