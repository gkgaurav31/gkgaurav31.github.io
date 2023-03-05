---
layout: post
title: Cheapest Flights Within K Stops
date: 2023-03-04 20:00 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150 important"
categories: "neetcode150"
---

### Problem Description

There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

[leetcode](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)

### Solution

This can be solved by using Bellman Ford. Here, we will performn the relaxation of the nodes k+1 times as we can have maximum k+1 edges (k stops) between the source and destination.

[NeetCode YouTube](https://www.youtube.com/watch?v=5eIK3zUdYmE)

```java
class Solution {

    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        
        //init dist/cost array
        int[] distance = new int[n];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[src] = 0;

        //temo dist/cost array
        int[] tempDistance = new int[n];
        Arrays.fill(tempDistance, Integer.MAX_VALUE);
        tempDistance[src] = 0;

        //use max k+1 edges (k stops)
        for(int i=1; i<=k+1; i++){
            
            //update the dist/cost for each reachable node
            for(int j=0; j<flights.length; j++){
                
                int u = flights[j][0];
                int v = flights[j][1];
                int w = flights[j][2];

                //if the current source is reachable 
                //and the distance from the current node is less than previously calculated value
                //then update the current min distance in the temp distance array
                if(distance[u] != Integer.MAX_VALUE && distance[u] + w < tempDistance[v])
                    tempDistance[v] = Math.min(distance[v], distance[u] + w);
                    
            }

            //copy values from temp array to original array
            for(int x=0; x<n; x++) distance[x] = tempDistance[x];
            
        }

        //if destination is not reachable, return -1
        if(distance[dst] == Integer.MAX_VALUE) return -1;

        return distance[dst];
    }
    
}
```
