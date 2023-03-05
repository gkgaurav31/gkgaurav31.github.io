---
layout: post
title: Swim in Rising Water
date: 2023-03-05 15:20 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

You are given an n x n integer matrix grid where each value ```grid[i][j]``` represents the elevation at that point (i, j).

The rain starts to fall. At time t, the depth of the water everywhere is t. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the least time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).
[leetcode](https://leetcode.com/problems/swim-in-rising-water/description/)

### Solution

We can solve this by making using of Dijkstra's Algorithm and mofifying it a bit. We are not looking at the shortest distance to reach destination Node, rather, we want to reduce the maximum level present in a path which connects source to the destination.

```java
class Solution {
    
    public int swimInWater(int[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        //to mark if a node has been visited before
        int[][] visited = new int[n][m];

        //Min Heap: based on level of the nodes
        PriorityQueue<Node> pq = new PriorityQueue<>( (p1,p2) -> p1.level - p2.level );

        //init: add the source node
        pq.add(new Node(grid[0][0], 0, 0));

        //while minheap is not empty
        while(!pq.isEmpty()){

            //get the Node with minimum level
            Node current = pq.poll();

            //mark that node as visited
            visited[current.row][current.column] = 1;

            //if that Node is the destination, we can return its level
            if(current.row == n-1 && current.column == m-1) return current.level;

            //Otherwise, look at the next 4 directions
            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};

            //visit neighbours in the 4 directions
            for(int k=0; k<4; k++){
                
                //next row
                int i = current.row + x[k];

                //next column
                int j = current.column + y[k];

                //check if they are within bounds and not already visited
                if(i>=0 && j>=0 && i<n && j<m && visited[i][j] == 0){

                    //the level of this Node will be max between its level and the maximum level which we have calculated to reach this Node, which is stored in current.level 
                    int level = Math.max(current.level, grid[i][j]);

                    //add it to minHeap
                    pq.add(new Node(level, i, j));
                }
                

            }


        }
        
        return -1;

    }

}

class Node{

    int level;
    int row;
    int column;
    
    Node(int l, int r, int c){
        level = l; 
        row = r;
        column = c;
    }
    
}
```
