---
layout: post
title: Minimal Spanning Tree
date: 2023-02-25 22:05 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs important"
categories: "graphs"
---

### Problem Description

Given a weighted, undirected and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/minimum-spanning-tree/1)

### Solution

Here we have used Kruskal's algorithm.

```java
class Solution{
    
 static int spanningTree(int V, int E, int edges[][]){
     
     //sort based on weight of the edges (ascending)
     //this is obvious because we need to form a tree which min weight
     Arrays.sort(edges, (o1, o2) -> o1[2] > o2[2]?1: (o1[2] < o2[2]?-1:0));
     
     //init parent array -> initially with no edges, every node will be its own parent
     int[] parent = new int[V];
     for(int i=0; i<parent.length; i++){
         parent[i] = i;
     }
     
     //init total cost
     int cost = 0;
     
     //check for each edge if it can be included
     for(int i=0; i<E; i++){
         
         //get the nodes forming the current edge
         int u = edges[i][0];
         int v = edges[i][1];
         
         //if both the nodes are part of the same connected component, we cannot choose it as they will form a cycle if included
         //if they have different leaders, they can be merged as a single component. pick that edge and add the cost
         if(union(u, v, parent)){
             cost += edges[i][2];
         }
         
     }
     
     return cost;
     
 }
 
 public static int find(int x, int[] parent){
     
     while(parent[x] != x){
         x = parent[x];
     }
     
     return parent[x];
     
 }
 
 public static boolean union(int x, int y, int[] parent){
     
     //if parent of x and y are same, union is not needed so return false
     if(find(x, parent) == find(y, parent)) return false;
     
     //else, merge them and return true
     //to merge them, we need to change the parent of the leader of one component as the leader of second component
     parent[find(x, parent)] = find(y, parent);
     
     return true;
     
 }
 
}
```

### Optimization

In the previous code, find(node) method which gets the leader of any node can take O(height of tree) => O(N) in the worst case. If we could somehow reduce the height of the tree, that would help in reducing the time taken to get the leader. While finding the leader, we can keep changing the leaders of intermediate nodes in such a way that they directly point to the leader of that connected component. So the next time we try to find the leader, it will take O(1) time. The only change will be in the find(node) method -

```java
 public static int find(int x, int[] parent){
    
    //if parent of x is x, then return it
    if(parent[x] == x){
        return parent[x];
    }
    
    //else update it's parent so point to the leader directly
    parent[x] = find(parent[x], parent);
    
    //return updated parent[x]
    return parent[x];
 }
```
