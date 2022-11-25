---
layout: post
title: Depth First Search
date: 2022-11-25 23:46 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs"
categories: "graphs"
---

### Problem Description

You are given a connected undirected graph. Perform a Depth First Traversal of the graph.
Note: Use a recursive approach to find the DFS traversal of the graph starting from the 0th vertex from left to right according to the graph.  

[geeksforgeeks](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)

```java
class Solution {
    // Function to return a list containing the DFS traversal of the graph.
    public ArrayList<Integer> dfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        
        boolean[] visited = new boolean[V+1];
        
        ArrayList<Integer> ans = new ArrayList<>();
        
        dfs(0, visited, adj, ans);
        
        return ans;
        
    }
    
    
    public void dfs(int s, boolean[] visited, ArrayList<ArrayList<Integer>> adj, ArrayList<Integer> ans){
        
        if(visited[s] == true) return;
        
        visited[s] = true;
        ans.add(s);
        
        ArrayList<Integer> neighbours = adj.get(s);
        
        for(int i=0; i<neighbours.size(); i++){
            dfs(neighbours.get(i), visited, adj, ans);
        }
        
    } 
    
}
```
