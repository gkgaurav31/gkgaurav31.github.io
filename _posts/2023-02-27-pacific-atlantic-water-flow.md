---
layout: post
title: Pacific Atlantic Water Flow
date: 2023-02-27 20:36 +0530
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where ```heights[r][c]``` represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

[leetcode](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/water_flow.png)

```java
class Solution {

    public List<List<Integer>> pacificAtlantic(int[][] heights) {

        int n = heights.length; 
        int m = heights[0].length; 

        //visited matrix for pacific and atlantic ocean        
        int[][] pacific = new int[n][m];
        int[][] atlantic = new int[n][m];

        //mark top row and left column as visited for pacific ocean
        //mark right column and bottom row as visited for atlantic ocean
        for(int c=0; c<m; c++){
            pacific[0][c] = 1;
            atlantic[n-1][c] = 1;
        }
        for(int r=0; r<n; r++){
            pacific[r][0] = 1;
            atlantic[r][m-1] = 1;
        }

        //apply multi-source BFS, in which the source is all the cells adjacent to the ocean
        //one thing to observe here is that we can reverse the logic of visiting the next call
        //that is, we will go to the neighboring cell only if its value is more (has a higher height)
        //we are doing this because, if the adjacent cell has a higher/equal height, only then the water will flow to the current cell
        visitOcean(heights, pacific);
        visitOcean(heights, atlantic);
        
        //If the cell [i,j] was visited in both pacific[][] and atlantic[][], it means that the water can flow from it to both the oceans
        //So, include it in the answer
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(pacific[i][j] == 1 && atlantic[i][j] == 1){
                    List<Integer> t = new ArrayList<>();
                    t.add(i); t.add(j);
                    ans.add(t);
                }
            }
        }

        return ans;

    }

    //breadth first search
    public int[][] visitOcean(int[][] heights, int[][] visited){

        int n = heights.length; 
        int m = heights[0].length;

        Queue<Pair> pq = new LinkedList<>();

        //add source nodes to the queue (we are using multi-source BFS)
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(visited[i][j] == 1){
                    pq.add(new Pair(i, j));
                }
            }
        }

        //keep taking our one cell/node at a time and see if the adjacent nodes/cells can be visited
        //we will visit them only if they have equal or more height 
        while(!pq.isEmpty()){

            Pair p = pq.poll();

            //direction helper
            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};
            
            //go to the 4 directions one by one
            for(int k=0; k<4; k++){
                
                //next X and Y positions
                int x2 = p.x + x[k];
                int y2 = p.y + y[k];

                //check if they are valid and if the height is equal or more than the current cell's height 
                //and they must not have been visited before
                if(x2>=0 && x2<n && y2>=0 && y2<m && heights[x2][y2] >= heights[p.x][p.y] && visited[x2][y2] != 1){
                    visited[x2][y2] = 1;
                    pq.add(new Pair(x2, y2));
                }

            }

        }

        return visited;

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
