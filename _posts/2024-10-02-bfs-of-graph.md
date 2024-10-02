---
layout: post
title: BFS of graph (geeksforgeeks - SDE Sheet)
date: 2024-10-02 17:41 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a directed graph. The task is to do Breadth First Traversal of this graph starting from 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1?page=9)

## SOLUTION

We use a queue to explore nodes level by level, marking each node as visited once it's processed. For each node, we visit its unvisited neighbors, mark them as visited, and add them to the queue.

```java
class Solution {

    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {

        ArrayList<Integer> list = new ArrayList<>();

        Queue<Integer> q = new LinkedList<>();
        q.add(0);

        boolean[] visited = new boolean[V];
        visited[0] = true;

        while(!q.isEmpty()){

            int current = q.poll();

            list.add(current);

            ArrayList<Integer> neighbors = adj.get(current);

            for(Integer neighbor: neighbors){

                if(!visited[neighbor]){

                    visited[neighbor] = true;
                    q.add(neighbor);

                }

            }

        }

        return list;

    }

}
```
