---
layout: post
title: Topological sort (geeksforgeeks - SDE Sheet)
date: 2024-10-12 18:40 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an adjacency list for a Directed Acyclic Graph (DAG) where adj_list[i] contains a list of all vertices j such that there is a directed edge from vertex i to vertex j, with V vertices and E edges, your task is to find any valid topological sorting of the graph.

In a topological sort, for every directed edge u -> v, u must come before v in the ordering.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/topological-sort/1?page=9)

## SOLUTION

To implement Topological Sort in a Directed Acyclic Graph, we can use `Kahn's Algorithm`, which involves calculating the in-degrees of all vertices to identify which vertices can be processed first.

We start by creating an array to store the in-degrees, which counts the number of edges directed into each vertex. Then, we initialize a queue with all vertices that have zero in-degrees, as these vertices do not depend on any other vertices.

As we process each vertex from the queue, we add it to the result list and decrease the in-degrees of its neighboring vertices. If any neighbor's in-degree becomes zero, we enqueue it for processing.

```java
class Solution
{

    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj)
    {

        // store the in-degrees of all vertices
        int[] indegree = new int[V];

        for (int i = 0; i < V; i++) {
            for (Integer j : adj.get(i)) {
                indegree[j]++;
            }
        }

        // Initialize a queue to store vertices with zero in-degrees
        Queue<Integer> q = new LinkedList<>();

        // Add all vertices with in-degree 0 to the queue
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.add(i);
            }
        }

        // List to store the topological order of vertices
        List<Integer> list = new ArrayList<>();

        // Process vertices in the queue
        while (!q.isEmpty()) {

            // Dequeue a vertex
            int x = q.poll();

            // Add the vertex to the result
            list.add(x);

            // Update in-degrees of adjacent vertices
            for (int n : adj.get(x)) {

                indegree[n]--;

                // Add to queue if in-degree becomes zero
                if (indegree[n] == 0) {
                    q.add(n);
                }

            }
        }

        return list.stream().mapToInt(Integer::intValue).toArray();
    }
}
```
