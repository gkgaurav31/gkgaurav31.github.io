---
layout: post
title: Circle of strings (geeksforgeeks - SDE Sheet)
date: 2024-10-21 19:35 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr of lowercase strings, determine if the strings can be chained together to form a circle.
A string X can be chained together with another string Y if the last character of X is the same as the first character of Y. If every string of the array can be chained with exactly two strings of the array(one with the first character and the second with the last character of the string), it will form a circle.

For example, for the array arr[] = {"for", "geek", "rig", "kaf"} the answer will be Yes as the given strings can be chained as "for", "rig", "geek" and "kaf"

[geeksforgeeks](https://www.geeksforgeeks.org/problems/circle-of-strings4530/1?page=5)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=g9pW9nwO_I8)

The logic of the solution is based on treating the strings as a graph where each string connects its starting character to its ending character.

We want to check if all the strings can be arranged in a way that forms a circle. To do this, we first check if each character (a-z) has the same number of strings starting with it as ending with it, because this ensures the strings can chain together.

Next, we make sure all the characters that have outgoing strings are connected, meaning you can travel between them, ensuring they form a single connected chain. If both these conditions are met, the strings can be arranged to form a circle. If not, it's impossible to do so.

```java
class Solution {

    public int isCircle(String arr[]) {

        int n = arr.length;

        // Create an adjacency list for the graph representation of 26 letters (a-z)
        List<Integer>[] g = new List[26];
        for(int i = 0; i < 26; i++)
            g[i] = new ArrayList<>();

        // Arrays to track in-degrees and out-degrees for each character (a-z)
        int[] indegree = new int[26];
        int[] outdegree = new int[26];

        // Build the graph
        for(int i = 0; i < n; i++){

            // Get the start (u) and end (v) characters of the current string
            int u = arr[i].charAt(0) - 'a';
            int v = arr[i].charAt(arr[i].length() - 1) - 'a';

            // Increment in-degree and out-degree for corresponding characters
            indegree[v]++;
            outdegree[u]++;

            // Add a directed edge from u to v in the graph
            g[u].add(v);

        }

        // Check if the in-degree equals the out-degree for every vertex
        // If not, return 0 as it's not possible to form a circle
        for(int i = 0; i < 26; i++){
            if(indegree[i] != outdegree[i])
                return 0;
        }

        // Find a valid starting node (a node with outgoing edges)
        int startNode = -1;
        for(int i = 0; i < 26; i++){
            if(outdegree[i] > 0){
                startNode = i;
                break;
            }
        }

        // If no valid starting node found, return 0 (no edges to form a circle)
        if (startNode == -1)
                return 0;

        // Array to keep track of visited nodes during DFS traversal
        boolean[] visited = new boolean[26];

        // Perform a DFS from the starting node to visit all connected vertices
        dfs(g, visited, startNode);

        // After DFS, check if all nodes with outgoing edges were visited
        // If any node was not visited, it means the graph is not strongly connected
        for(int i = 0; i < 26; i++){
            if(outdegree[i] > 0 && !visited[i])
                return 0;
        }

        return 1;

    }

    public void dfs(List<Integer>[] g, boolean[] visited, int node){
        if(visited[node])
            return;

        visited[node] = true;

        // Recursively visit all the adjacent nodes (edges from current node)
        for(int n: g[node]){
            dfs(g, visited, n);
        }
    }
}
```
