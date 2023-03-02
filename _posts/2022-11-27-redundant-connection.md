---
layout: post
title: Redundant Connection
date: 2022-11-27 20:56 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.

Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.
[leetcode](https://leetcode.com/problems/redundant-connection/description/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/redundant_connection.png)

find() -> get the "leader" of that set/group  
union() -> combine the sets/groups, which means, one of the leaders can be made as a child of another or vise-versa  

```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        
        //A tree has (nodes-1) edges. There is one extra edge which is creating the cycle
        int n = edges.length;

        //Initialize: we will assume that each node's parent is itself
        int[] parent = new int[n+1];
        for(int i=1; i<parent.length; i++){
            parent[i] = i;
        }

        for(int i=0; i<edges.length; i++){

            int a = edges[i][0];
            int b = edges[i][1];

            if(find(parent, a) == find(parent, b)){
                return edges[i];
            }else{
                union(parent, find(parent, a), find(parent, b));
            }

        }

        return new int[]{};

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

### OPTIMIZATION

We can optimize the find(x) method to O(1) -

See: [Minimal Spanning Tree]({% post_url 2023-02-25-minimal-spanning-tree %})

```java
int find(int x, int[] leader){

    if(leader[x] == x) return x;

    while(leader[x] != x){
        x = leader[x];
    }

    leader[x] = find(leader[x], leader);

    return leader[x];

}
```
