---
layout: post
title: Evaluate Division
date: 2023-08-15 22:24 +0530
author: "Gaurav Kumar"
tags: "java graphs leetcode leetcode150"
categories: "graphs"
---
## PROBLEM DESCRIPTION

You are given an array of variable pairs equations and an array of real numbers values, where equations[i] = [Ai, Bi] and values[i] represent the equation Ai / Bi = values[i]. Each Ai or Bi is a string that represents a single variable.

You are also given some queries, where queries[j] = [Cj, Dj] represents the jth query where you must find the answer for Cj / Dj = ?.
Return the answers to all queries. If a single answer cannot be determined, return -1.0.
Note: The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.
Note: The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.

[leetcode](https://leetcode.com/problems/evaluate-division/)

## SOLUTION

The important observation is that, if the value from A -> B is x, then the value from B -> A will be 1/x. Also, if A -> B is x and B -> C is y, then A -> C will be x multiplied by y. To solve this we can therefore create a directed graph and use BFS from source to destination while keeping the current value which needs to be multiplied based on the "weight" of that edge from source to destination.  

Another small thing to take care of in this problem is that the String AB and BA mean the same key. So, we can store the sorted value of that string while using the directed weighted graph.

```java
class Solution {
    
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {

        // Build directed graph
        //A -> B with weight from values[]
        //B -> A with 1/weight
        Map<String, List<Pair>> graph = new HashMap<>();

        // Populate the graph with given equations and values
        for (int i = 0; i < equations.size(); i++) {
            String x = sorted(equations.get(i).get(0));  // Source variable
            String y = sorted(equations.get(i).get(1));  // Destination variable
            double val = values[i];  // Value of the equation

            // Add the edge from x to y with weight val
            graph.computeIfAbsent(x, k -> new ArrayList<>()).add(new Pair(y, val));

            // Add the edge from y to x with weight 1/val
            graph.computeIfAbsent(y, k -> new ArrayList<>()).add(new Pair(x, 1.0 / val));
        }

        // Array to store results for each query
        double[] ans = new double[queries.size()];

        // Evaluate each query
        for (int i = 0; i < queries.size(); i++) {
            ans[i] = calculateValue(graph, sorted(queries.get(i).get(0)), sorted(queries.get(i).get(1)), 1.0, new HashSet<String>());
        }

        return ans;
    }

    // Recursive function to calculate the value of a query
    public double calculateValue(Map<String, List<Pair>> graph, String source, String destination, double total, Set<String> visited) {

        // If we've already visited this node -1
        if (visited.contains(source)) return -1.0;

        // If the source or destination node is not in the graph, return -1.0
        if (!graph.containsKey(source) || !graph.containsKey(destination)) {
            return -1.0;
        }

        // Get the neighbors of the current source node
        List<Pair> neighbors = graph.get(source);

        // Iterate through neighbors to find a valid path
        for (Pair p : neighbors) {

            String d = p.destination;  // Neighbor destination
            double w = p.weight;       // Weight of the edge

            // If we've reached the destination, return the calculated value
            if (d.equals(destination)) {
                return total * w;
            }

            // Mark the current source as visited
            visited.add(source);

            // Recurse to find a path from the neighbor to the destination
            double result = calculateValue(graph, d, destination, total * w, visited);

            // If a valid result is found, return it
            if (result != -1.0) {
                return result;
            }
        }

        // No valid path found, return -1.0
        return -1.0;
    }

    // Method to sort characters in a string and return the sorted result
    public static String sorted(String input) {
        char[] charArray = input.toCharArray();
        java.util.Arrays.sort(charArray);
        return new String(charArray);
    }

}

// Class to represent an edge (pair) in the graph
class Pair {

    String destination;
    double weight;

    Pair(String destination, double weight){
        this.destination = destination;
        this.weight = weight;
    }

}

```
