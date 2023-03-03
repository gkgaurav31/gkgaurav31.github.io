---
layout: post
title: Min Cost to Connect All Points
date: 2023-03-03 23:58 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

- Every adjacent pair of words differs by a single letter.
- Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
- sk == endWord

Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

[leetcode](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)

### Solution

The minimum cost to connect all points is basically about finding the minimum spanning tree with weights defined by |x1-x2| + |y1-y2|

```java
class Solution {
    
    public int minCostConnectPoints(int[][] points) {

        //number of nodes/vertices
        int n = points.length;

        //number of edges = nC2 -> n*(n-1)/2 
        int e = n*(n-1)/2;

        //to store all edges with their weights
        int[][] edges = new int[e][3];
        
        //to add edge info at index idx in edges array
        int idx=0;
        
        //loop through all the points combinations
        //each combination can form an edge with weight = |x1-x2| + |y1-y2|
        for(int i=0; i<n; i++){

            //we start with j=i+1 to avoid duplicate (point A to point B is same as point B to point A)
            for(int j=i+1; j<n; j++){

                //calculate the weight of that edge
                int w = Math.abs(points[i][0]-points[j][0]) + Math.abs(points[i][1]-points[j][1]);

                //store edge info
                edges[idx][0] = i;
                edges[idx][1] = j;
                edges[idx][2] = w;
                
                //increment edge count
                idx++;

            }
            
        }

        //the minimum cost is the MST 
        return spanningTree(n, e, edges);

    }

    //Minimum Spanning Tree
    static int spanningTree(int V, int E, int edges[][]){
        
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
