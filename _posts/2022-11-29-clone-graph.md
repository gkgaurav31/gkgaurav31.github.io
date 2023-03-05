---
layout: post
title: Clone Graph
date: 2022-11-29 22:56 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```

[leetcode](https://leetcode.com/problems/clone-graph/description/)

### Solution

We can use a HashMap to store a Node and its clone. Then we can use DFS to visit each node.

[NeetCode | YouTube](https://www.youtube.com/watch?v=mQeF6bN8hMk)

```java
class Solution {
    
    public Node cloneGraph(Node node) {
        return cloneGraphHelper(node, new HashMap<>());
    }

    //DFS
    public Node cloneGraphHelper(Node node, Map<Node, Node> map){

        //If the node itself is null, return null
        if(node == null) return null;

        //If the node was already clone before, it would be present in the HashMap, so return it
        if(map.containsKey(node)) return map.get(node);

        //Otherwise, create a new clone and put it to the HashMap to use later if needed
        Node clone = new Node(node.val);
        map.put(node, clone);

        //We will also need to clone its neighbors
        //For each neighbor of the original node, use DFS and keep cloning them and adding that clone to the current clone's neighbors
        for(Node x: node.neighbors){
            clone.neighbors.add(cloneGraphHelper(x, map));
        }

        //return the cloned node
        return clone;

    }
}

/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/
```
