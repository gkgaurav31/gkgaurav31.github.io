---
layout: post
title: Breadth First Search
date: 2022-11-25 00:01 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs"
categories: "graphs"
---

## Problem Description

Given a directed graph. The task is to do Breadth First Traversal of this graph starting from 0.

### Solution

```java
class Solution {
    
    // Function to return Breadth First Traversal of given graph.
    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        
        Queue<Integer> q = new LinkedList<>();

        //initialize the queue by adding the source node, which is 0 in this case
        q.add(0);
        
        //visited array to track the nodes which have been already visited
        boolean[] visited = new boolean[V];

        //mark node 0 as visited
        visited[0] = true;

        //bfs answer list. add 0 to it        
        ArrayList<Integer> bfs = new ArrayList<>();
        bfs.add(0);
        
        //while queue is not empty
        while(!q.isEmpty()){
            
            //take out the node from queue
            int n = q.poll();

            //get the neighbours of that node
            ArrayList<Integer> neighbours = adj.get(n);
            
            //loop through all the neighbours and check if they have been visited
            for(Integer x: neighbours){

                //if the neighbour node has not been visited
                if(visited[x] == false){

                    //add to answer bfs list
                    bfs.add(x);

                    //add to queue so that we can later visit its neighbours
                    q.add(x);

                    //mark it as visited so that we don't visit it again
                    visited[x] = true;
                }
            }
            
        }
        
        return bfs;    
        
    }
}
```
