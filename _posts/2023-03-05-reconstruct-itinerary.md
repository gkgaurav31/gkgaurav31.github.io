---
layout: post
title: Reconstruct Itinerary
date: 2023-03-05 14:26 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150 important"
categories: "neetcode150"
---

### Problem Description

You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

- For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

[leetcode](https://leetcode.com/problems/reconstruct-itinerary/description/)

### Solution

The idea to solve this problem is simple but the implementation is a bit tricky. Here, we have used DFS to check the path. It's possible that the next node (destination) that we pick up will not lead to any solution, in which case, we need to backtrack and check the next destination.

A good test case:
> [["JFK","KUL"],["JFK","NRT"],["NRT","JFK"]]

The result for this test case will be:
>["JFK","NRT","JFK","KUL"]

```java
class Solution {
    
    public List<String> findItinerary(List<List<String>> tickets) {

        //Create an adjacency list using tickets
        //<String, List<String>> : <Source, Neighbours>
        Map<String, List<String>> g = new HashMap<>();

        for(int i=0; i<tickets.size(); i++){

            String source = tickets.get(i).get(0);
            String destination = tickets.get(i).get(1);

            if(g.get(source) == null){
                List<String> t = new ArrayList<>();
                t.add(destination);
                g.put(source, t);
            }else{
                g.get(source).add(destination);
            }

        }

        //We need to return the path which comes first in lexical order
        //So, sort the list of neighbours
        for(java.util.Map.Entry e: g.entrySet()){
            Collections.sort((List<String>)e.getValue());
        }

        //DFS with backtracking
        //The number of nodes in path must be number of tickets + 1
        //Number of tickets represent edges
        travel(g, new ArrayList<String>(Arrays.asList("JFK")), "JFK", tickets.size()+1);

        //Return the first path from all pathsPossible
        return pathsPossible.get(0);

    }

    List<List<String>> pathsPossible = new ArrayList<>();

    public List<String> travel(Map<String, List<String>> g, List<String> path, String source, int size){

        //We can remove this line to get a list of all possible paths
        //This leads to memory out of limit though
        //To handle that, we will just return when we've found our first path which is what we ultimately need to return
        if(pathsPossible.size() > 0) return path;

        //base consition
        //if all edges have been used
        if(path.size() == size){
            pathsPossible.add(new ArrayList<>(path));
            return path;
        }

        //get the neighbours of current source
        List<String> neighbours = g.get(source);

        //if there are no neighbours, return
        if(neighbours == null || neighbours.size() == 0) return path;

        //otherwise, try out each neighbour to see if that path is possible
        for(int i=0; i<neighbours.size(); i++){
            
            //store next node to visit
            String next = neighbours.get(i);
            
            //add it to the path
            path.add(next);

            //remove it from neighbours so that we don't use that edge (which represents ticket) again
            g.get(source).remove(next);

            //call DFS recursively using the this node
            travel(g, path, next, size);
            
            //backtrack
            //add the node back to it's sorted order
            g.get(source).add(i, next); 

            //remove the node from path and try other nodes
            path.remove(path.size()-1);

        }

        return path;

    }

}
```
