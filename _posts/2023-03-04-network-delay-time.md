---
layout: post
title: Network Delay Time
date: 2023-03-04 14:21 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

[leetcode](https://leetcode.com/problems/network-delay-time/description/)

### Solution

This can be solved using Dijkstra's algorithm (shortest path to all nodes)

```java
class Solution {
    
    public int networkDelayTime(int[][] times, int n, int k) {
        
        //create adjacency list (directed weighted graph)
        //edge -> times[0] to times[1] with weight as times[2]
        List<Node>[] g = new ArrayList[n+1];
        for(int i=0; i<g.length; i++) g[i] = new ArrayList<>();

        for(int i=0; i<times.length; i++){

            int u = times[i][0];
            int v = times[i][1];
            int w = times[i][2];
            
            g[u].add(new Node(v, w));

        }

        //initilize delay array to store the time taken to reach any node from the source node k
        int[] delay = new int[n+1];

        //the time taken for source node will be 0
        //for others, set it to +infinity
        Arrays.fill(delay, Integer.MAX_VALUE);
        delay[k] = 0;

        //we need to get the minimum weighted edge which are possible from the currently visited nodes
        //for this we can use minHeap sorted by weights (time in this case)
        PriorityQueue<Pair> pq = new PriorityQueue<>((p1, p2) -> p1.time - p2.time);

        //add the source node with time 0 to min Heap
        pq.add(new Pair(0, k));

        //while queue is not empty
        while(!pq.isEmpty()){
            
            //get the node with min delay
            Pair p = pq.poll();

            //get its neighbours
            List<Node> neighbours = g[p.node];

            //for all neighbours, update the time needed for signal to reach 
            for(int i=0; i<neighbours.size(); i++){
                
                Node current = neighbours.get(i);

                int tempDelay = current.weight + p.time;

                //if the current calculated delay is lesser, update it in delay array 
                //and add that node to minHeap so that it can be used to reach other nodes 
                if(tempDelay < delay[current.node]){
                    delay[current.node] = tempDelay;
                    pq.add(new Pair(tempDelay, current.node));
                }
                //if the tempDelay was more, that indicates that the signal had already reached before and we can continue 

            }


        }

        //the min time needed will be the max delay in delay array
        int minTimeNeeded = Integer.MIN_VALUE;
        for(int i=1; i<delay.length; i++){
            if(delay[i] == Integer.MAX_VALUE) return -1;
            minTimeNeeded = Math.max(minTimeNeeded, delay[i]);
        }

        return minTimeNeeded;

    }

}

class Node{

    int node;
    int weight;
    
    Node(int n, int w){
        node = n;
        weight = w;
    }

}

class Pair{

    int time;
    int node;

    Pair(int t, int n){
        time = t;
        node = n;
    }

}

```
