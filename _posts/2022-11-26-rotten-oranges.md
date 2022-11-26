---
layout: post
title: Rotten Oranges
date: 2022-11-26 16:06 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

You are given an m x n grid where each cell can have one of three values:

- 0 representing an empty cell,
- 1 representing a fresh orange, or
- 2 representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.  

[leetcode](https://leetcode.com/problems/rotting-oranges/description/)

### Solution

The rotting of oranges will start from every exiting rotten orange in the given matrix. So, we will need to start from all those positions and keep visiting its neighbours. While visiting the neighbours, we will need to keep a track of time when they rot. This will be 1 more than the cell from which it got rot. We can create a new matrix to keep a track of time to rot. This can also be used to check if that node/cell has been visited. The idea is basically to apply __Multi Source BFS__.

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        
        int n = grid.length;
        int m = grid[0].length;

        //Queue to add neighbours while visiting the nodes/cells
        Queue<Pair> queue = new LinkedList<>();


        //time array to track at what time a certain orange will be rotten
        int[][] time = new int[n][m];

        //initialize time to rot as -1 (not visited)
        for(int i=0; i<n; i++){
            Arrays.fill(time[i], -1);
        }

        //add rotten oranges in the queue and mark them visited
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){

                //if grid[i][j] is 2, that orange is rotten as per the problem
                if(grid[i][j] == 2){

                    //add it to the queue
                    queue.add(new Pair(i, j));

                    //mark the time as 0 (marking visited)
                    //making it 0 will also help in calculating the time for neighboring oranges
                    time[i][j] = 0;
                }
            }
        }

        //variable to track the max time any fresh orange took to rot
        int maxTime = 0;

        //direction helper to visit cells towards top, bottom, left, right
        int[] x = {0, 0, 1, -1};
        int[] y = {1, -1, 0, 0};

        //while queue is not empty, keep visiting the neighbours of rotten oranges and make then rot
        //each time, update the time to rot in the time matrix
        while(!queue.isEmpty()){
            
            //get the rotten orange from the queue
            Pair p = queue.poll();

            //get it's ith and jth indices
            int i = p.x;
            int j = p.y;

            //visit the neighboring nodes/cells            
            for(int k=0; k<4; k++){
                
                //calculate the next indicates to be visited
                int a = i+x[k];
                int b = j+y[k];

                //check if they are within the bounds
                //they have not been visited -> time[a][b] == -1
                //and they are not empty -> grid[a][b] != 0
                if(a>=0 && b>=0 && a<n && b<m && time[a][b] == -1 && grid[a][b] != 0){

                    //add the orange to queue so that its neighbours can be visited                    
                    queue.add(new Pair(a, b));

                    //mark the time to rot (it will be 1 more than the time to rot from the previous cell)
                    time[a][b] = time[i][j] + 1;

                    //update maxTime to rot
                    maxTime = Math.max(maxTime, time[a][b]);

                }

            }

        }

        //There is a chance that not all the oranges have been rotten, in which case we can return -1
        if(allRotted(grid, time)){
            return maxTime;
        }else{
            return -1;
        }

    }

    public boolean allRotted(int[][] grid, int[][] time){
        for(int i=0; i<time.length; i++){
            for(int j=0; j<time[0].length; j++){
                if(grid[i][j] == 1 && time[i][j] == -1) return false;
            }
        }

        return true;
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
