---
layout: post
title: Walls and Gates
date: 2023-02-28 21:53 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

You are given an m x n grid rooms initialized with these three possible values.

- -1 A wall or an obstacle.
- 0 A gate.
- INF Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

[leetcode](https://leetcode.com/problems/walls-and-gates/description/)

### Solution

This can be solved using multi-source BFS in which the source will be the gates.

```java
class Solution {

    private static final int INF = 2147483647;

    public void wallsAndGates(int[][] rooms) {
        
        int n = rooms.length;
        int m = rooms[0].length;

        Queue<Pair> q = new LinkedList<>();

        //add all cells with value 0 (which represent the gates) to the queue
        //we will use multi-source BFS to check the distance or other reachable cells
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(rooms[i][j] == 0) q.add(new Pair(i, j));
            }
        }

        while(!q.isEmpty()){
            
            Pair p = q.poll();

            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};

            for(int k=0; k<4; k++){

                int x2 = p.x + x[k];
                int y2 = p.y + y[k];
                
                //if the next cell is within the range and its value is INF, which means it's an empty room which has not been visited yet
                if(x2 >=0 && y2 >=0 && x2<n && y2<m && rooms[x2][y2] == INF){
                    rooms[x2][y2] = 1 + rooms[p.x][p.y];
                    q.add(new Pair(x2, y2));
                }

            }

        }

    }
}

class Pair{

    int x;
    int y;

    Pair(int x, int y){
        this.x = x;
        this.y = y;
    }
    
}
```
