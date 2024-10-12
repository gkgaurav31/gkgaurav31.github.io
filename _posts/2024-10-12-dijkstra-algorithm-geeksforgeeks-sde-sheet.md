---
layout: post
title: Dijkstra Algorithm (geeksforgeeks - SDE Sheet)
date: 2024-10-12 22:05 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a weighted, undirected and connected graph of V vertices and an adjacency list adj where adj[i] is a list of lists containing two integers where the first integer of each list j denotes there is edge between i and j , second integers corresponds to the weight of that edge . You are given the source vertex S and You to Find the shortest distance of all the vertex's from the source vertex S. You have to return a list of integers denoting shortest distance between each node and Source vertex S.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1?page=9)

## SOLUTION

- Create a distance array to store the shortest distance from the source to each vertex, initializing all values to infinity (`Integer.MAX_VALUE`), except the source vertex which is set to 0.
- Use a **Priority Queue** to process vertices based on their current shortest distance (smallest distance first).

- Add the source vertex to the priority queue with a distance of 0.

- While the priority queue is not empty:

  - Remove the vertex with the smallest distance from the queue.
  - For each neighbor of this vertex:
    - Check if going through this vertex offers a shorter path to the neighbor.
    - If it does, update the neighbor's distance and add the neighbor to the queue with the new distance.

- Continue this process until all vertices have been processed.

```java
class Solution {

    static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {

        // Array to store the shortest distances from source to each vertex
        int[] distance = new int[V];

        // init: infinity (MAX_VALUE) to represent they are unreachable initially
        Arrays.fill(distance, Integer.MAX_VALUE);

        // Distance from source to itself is always 0
        distance[S] = 0;

        // Priority queue to process nodes by their current shortest distance (min-heap)
        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> a.distance - b.distance);

        // Add the source node to the priority queue with distance 0
        pq.add(new Node(S, 0));

        // While there are nodes in the priority queue to process
        while (!pq.isEmpty()) {

            // Extract the node with the smallest distance (the most promising node)
            Node node = pq.poll();

            // Current vertex (u) and its distance from the source
            int u = node.vertex;
            int d = node.distance;

            // Explore all neighbors of vertex u
            for (int i = 0; i < adj.get(u).size(); i++) {

                // Get the neighbor (adjacent vertex and the weight of the edge)
                ArrayList<Integer> neighbor = adj.get(u).get(i);

                int neighborVertex = neighbor.get(0);
                int neighborWeight = neighbor.get(1);

                // If a shorter path to neighborVertex is found, update it
                if (d + neighborWeight < distance[neighborVertex]) {

                    // Update the shortest distance to the neighborVertex
                    distance[neighborVertex] = d + neighborWeight;

                    // Add the neighbor to the priority queue with the updated distance
                    pq.add(new Node(neighborVertex, distance[neighborVertex]));

                }
            }
        }

        return distance;
    }
}

// Node class to represent a vertex and its distance
class Node {

    int vertex;
    int distance;

    Node(int vertex, int distance) {
        this.vertex = vertex;
        this.distance = distance;
    }
}
```
