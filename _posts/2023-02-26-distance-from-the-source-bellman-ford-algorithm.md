---
layout: post
title: Distance from the Source (Bellman-Ford Algorithm)
date: 2023-02-26 18:40 +0530
tags: "java geeksforgeeks graphs important"
categories: "graphs"
---

### Problem Description

Given a weighted, directed and connected graph of V vertices and E edges, Find the shortest distance of all the vertex's from the source vertex S.
Note: If the Graph contains a negative cycle then return an array consisting of only -1.

Your task is to complete the function bellman_ford( ) which takes a number of vertices V and an E-sized list of lists of three integers where the three integers are u,v, and w; denoting there's an edge from u to v, which has a weight of w and source node S as input parameters and returns a list of integers where the ith integer denotes the distance of an ith node from the source node.

If some node isn't possible to visit, then its distance should be 100000000(1e8). Also, If the Graph contains a negative cycle then return an array consisting of only -1.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)

### Solution

[Tech Dose YouTube](https://www.youtube.com/watch?v=FrLWd1tJ_Wc)

- This algorithm can help in detecting negative cycles (if that is present, it will not give us the right answer for the shortest path)
- It works well for Directed Graphs. For undirected graph, we can consider two way edges from (u->v) and (v->u). If that edge of the undirected graph has a -ve value, Bellman algorithm will not work because it will create a negative cycle.

```java
class Solution {
    
    static int[] bellman_ford(int V, ArrayList<ArrayList<Integer>> edges, int S) {
        
        //init -> distance[source node] = 0, rest +infinite or more than the max value possible
        int[] distance = new int[V];
        Arrays.fill(distance, 1001); // -1000 ≤ adj[i][j] ≤ 1000
        distance[S] = 0;
        
        //we need N-1 edges if there are N nodes in a connected graph
        for(int i=0; i<V; i++){
            
            //for the current edge, we know that the cost from u->v is w.
            //if the shortest path from source to u is distance[u], the shortest path from u to v will be:
            //distance[u] + w
            //since we are looking for minimum, pick in minimum between:
            //Min(distance[v], distance[u] + w);
            for(int j=0; j<edges.size(); j++){
                
                int u = edges.get(j).get(0);
                int v = edges.get(j).get(1);
                int w = edges.get(j).get(2);
                
                distance[v] = Math.min(distance[v], distance[u] + w);
                
            }
            
        }
        
        //save the shortest distance after V-1 iterations
        int[] d1 = new int[V];
            for(int i=0; i<d1.length; i++) d1[i] = distance[i];
        
        //if we add another edge, it will definitely form a cycle
        //if the shortest path for any of the destination nodes now decreases, we can say that it must have formed a -ve cycle
        //in that case we can return [-1] as per the problem
        for(int j=0; j<edges.size(); j++){
            
            int u = edges.get(j).get(0);
            int v = edges.get(j).get(1);
            int w = edges.get(j).get(2);
            
            distance[v] = Math.min(distance[v], distance[u] + w);
            
        }
        
        if(hasNegativeCycle(d1, distance)){
            return new int[]{-1};
        }
        
        //if any of the nodes are not reachable, update its distance to 10^8, as per the problem
        for(int i=0; i<d1.length; i++){
            if(d1[i] >= 1001) d1[i] = 100000000;
        }
        
        //return d1, which was calculated after V-1 iterations
        return d1;
        
    }
    
    static boolean hasNegativeCycle(int[] d1, int[] d2){
        
        for(int i=0; i<d1.length; i++){
            if(d2[i] < d1[i]) return true;
        }
        
        return false;
        
    }
    
}
```

### BETTER WAY TO CODE

We can reduce some extra space that we have used -

```java
class Solution {
    
    static int[] bellman_ford(int V, ArrayList<ArrayList<Integer>> edges, int S) {
        
        //init -> distance[source node] = 0, rest +infinite or more than the max value possible
        int[] distance = new int[V];
        Arrays.fill(distance, 100000000); // -1000 ≤ adj[i][j] ≤ 1000
        distance[S] = 0;
        
        //we need N-1 edges if there are N nodes in a connected graph
        for(int i=0; i<V; i++){
            
            //for the current edge, we know that the cost from u->v is w.
            //if the shortest path from source to u is distance[u], the shortest path from u to v will be:
            //distance[u] + w
            //since we are looking for minimum, pick in minimum between:
            //Min(distance[v], distance[u] + w);
            for(int j=0; j<edges.size(); j++){
                
                int u = edges.get(j).get(0);
                int v = edges.get(j).get(1);
                int w = edges.get(j).get(2);
                
                distance[v] = Math.min(distance[v], distance[u] + w);
                
            }
            
        }

        //if the cost reduces for any of the nodes for next iteratation (Vth time), there must be a -ve cycle        
        for(int j=0; j<edges.size(); j++){
            
            int u = edges.get(j).get(0);
            int v = edges.get(j).get(1);
            int w = edges.get(j).get(2);
            
            if(distance[u] + w < distance[v]){
                return new int[]{-1};
            }            
            
        }
        
        return distance;
        
    }
    
}
```
