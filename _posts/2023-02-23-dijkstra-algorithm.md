---
layout: post
title: Dijkstra Algorithm
date: 2023-02-23 23:20 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs important"
categories: "graphs"
---

### Problem Description

Given a weighted, undirected and connected graph of V vertices and an adjacency list adj where adj[i] is a list of lists containing two integers where the first integer of each list j denotes there is edge between i and j , second integers corresponds to the weight of that  edge . You are given the source vertex S and You to Find the shortest distance of all the vertex's from the source vertex S. You have to return a list of integers denoting shortest distance between each node and Source vertex S.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)

### Solution

```java
class Solution
{
    //function to find the shortest distance of all the vertices from the source vertex S.
    static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S)
    {
        
        //to store min distance from source node
        //initialize with INT_MAX assuming, which will be later decremented
        int[] distance = new int[adj.size()];
        Arrays.fill(distance, Integer.MAX_VALUE);
        
        //Min Heap -> to store the neighboring nodes and their distance. We will get the node with the least distance out of the min heap and explore its neighbours
        PriorityQueue<Pair> pq = new PriorityQueue<>((p1, p2) -> p1.distance - p2.distance);
        
        //init -> push the source node and update its time to 0
        pq.add(new Pair(0, S)); 
        distance[S] = 0;
        
        //while heap has nodes
        while(!pq.isEmpty()){
            
            //get the node with the least distance
            Pair current = pq.poll();
            int u = current.node;
            int d = current.distance;

            //If the distance turns out to be more than previously computed distance which is stored in distance array, it was already visited
            //In that case, we can simply continue with other nodes in the heap            
            if(d > distance[u]) continue; //already visited
            
            //If it was not already visited, get its neighbours and update their distance if it can be reduced even further
            ArrayList<ArrayList<Integer>> neighbours = adj.get(u);
            
            //Loop through all the neighboring nodes
            for(ArrayList<Integer> n: neighbours){
                
                int v = n.get(0); //destination node
                int w = n.get(1); //weight to reach from Node u to Node v
                
                //the min distance calculated for u was "d".
                //the weight from u to v is "w"
                //so min distance will be d+w (This may not be the min distance, so we will only update it if this is min than previously computed values, maybe from a diff node)
                int dist = d + w;
                
                //check if dist calculated is less than previously calculated distance
                //in that case, update the distance AND also add the new Pair to the min heap
                //IMPORTANT: We are not removing/updating the existing pair in the heap because it's a costly operation
                //adding this duplicate node with updated distance value is okay because we can easily check if the node was already visited before based on distance calculated -> if(d > distance[u]) continue; 
                if(dist < distance[v]){
                    distance[v] = dist;
                    pq.add(new Pair(dist, v));
                }
                
                
            }
            
        }
        
        return distance;
        
    }
}

class Pair{
    
    int distance;
    int node;
    
    Pair(int d, int n){
        this.distance = d;
        this.node = n;
    }
    
}
```
