---
layout: post
title: Graph Valid Tree
date: 2022-11-30 22:35 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.

[leetcode](https://leetcode.com/problems/graph-valid-tree/description/)

### Solution

Refer to the following question which shares similarity: [Redundant Connection]({% post_url 2022-11-27-redundant-connection %})

- The graph is not a valid tree if it has a cycle. We can use find and union approach to find that
- The graph can have multiple connected components. In that case it's not a valid tree. After checking for cycles, the parent array should have the same parent for all the nodes. If there are any two nodes with different parents, that means that graph must be disconnected. So, it's not a valid tree.

```java
class Solution {
    
    public boolean validTree(int n, int[][] edges) {

        //Initialize: we will assume that each node's parent is itself
        int[] parent = new int[n];
        for(int i=0; i<parent.length; i++){
            parent[i] = i;
        }

        for(int i=0; i<edges.length; i++){

            int a = edges[i][0];
            int b = edges[i][1];

            //If parent of both a and b are same, cycle exists => invalid tree
            if(find(parent, a) == find(parent, b)){
                return false;
            }else{
                union(parent, find(parent, a), find(parent, b));
            }

        }

        //at this point, parent of all nodes must be same 
        //if there are more than 1 parent, that means the graph has more than 1 connected component
        //in that case, it is not a valid tree, so we return false
        for(int i=1; i<parent.length; i++){
            if(find(parent, i) != find(parent, i-1)) return false;
        }

        return true;

    }

    public int find(int[] parent, int n){
        if(parent[n] == n) return n;
        return find(parent, parent[n]);
    }

    public void union(int[] parent, int u, int v){
        if(u <= v){
            parent[v] = u;
        }else{
            parent[v] = u;
        }
    }
}
```

### Optimized

[Graphs 101: Part 2]({% post_url 2023-02-21-graphs-101-part-2 %})

```java
class Solution {

    public boolean validTree(int n, int[][] edges) {

        int[] leader = new int[n];

        for(int i=0; i<n; i++) leader[i] = i;

        for(int i=0; i<edges.length; i++){

            if(!merge(edges[i][0], edges[i][1], leader)){
                return false;
            }

        }

        int commonLeader = find(0, leader);

        for(int i=1; i<n; i++){
            if(find(i, leader) != commonLeader) return false;
        }

        return true;
        
    }

    int find(int x, int[] leader){

        if(leader[x] == x) return x;

        while(leader[x] != x){
            x = leader[x];
        }

        leader[x] = find(leader[x], leader);

        return leader[x];

    }

    boolean merge(int x, int y, int[] leader){

        int leaderX = find(x, leader);
        int leaderY = find(y, leader);

        if(leaderX == leaderY) return false;

        leader[find(x, leader)] = find(y, leader);

        return true;
    }

}
```
