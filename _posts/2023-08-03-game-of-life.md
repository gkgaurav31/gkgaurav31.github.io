---
layout: post
title: Game of Life
date: 2023-08-03 20:03 +0530
author: "Gaurav Kumar"
tags: "java matrix leetcode leetcode150"
categories: "matrix"
---

## PROBLEM DESCRIPTION

According to Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an m x n grid of cells, where each cell has an initial state: live (represented by a 1) or dead (represented by a 0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

- Any live cell with fewer than two live neighbors dies as if caused by under-population.
- Any live cell with two or three live neighbors lives on to the next generation.
- Any live cell with more than three live neighbors dies, as if by over-population.
- Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the m x n grid board, return the next state.

[leetcode](https://leetcode.com/problems/game-of-life/)

## SOLUTION

### APPROACH 1: Using Extra Space

We calculate the number of neighbours for each cell and follow the rules specified in the problem to update the matrix. We use a temporary clone matrix to store our answer.

```java
class Solution {
    
    public void gameOfLife(int[][] board) {

        int n = board.length;
        int m = board[0].length;

        int[][] ans = new int[n][m];

        //temporary matrix to store the answer
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                ans[i][j] = board[i][j];
            }
        }

        for(int i=0; i<n; i++){

            for(int j=0; j<m; j++){
                
                //get the count of alive neighbours surrounding the current cell
                int c = countAliveNeighbours(board, i, j);

                //Any live cell with fewer than two live neighbors dies as if caused by under-population.
                if(board[i][j] == 1 && c < 2){
                    ans[i][j] = 0;
                }

                //Any live cell with more than three live neighbors dies, as if by over-population.
                if(board[i][j] == 1 && c > 3){
                    ans[i][j] = 0;
                }

                //Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
                if(board[i][j] == 0 && c == 3){
                    ans[i][j] = 1;
                }

            }

        }

        //update the original matrix
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                board[i][j] = ans[i][j];
            }
        }

    }

    public int countAliveNeighbours(int[][] board, int i, int j){

        int n = board.length;
        int m = board[0].length;

        //helper to move to all other directions from a given co-ordinate
        int[] x = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] y = {-1, 0, 1, -1, 1, -1, 0, 1};

        int alive = 0;

        for(int k=0; k<8; k++){

            int nextX = i + x[k];
            int nextY = j + y[k];

            //check for out of bounds
            if(nextX >= 0 && nextX < n && nextY >=0 && nextY < m){
                if(board[nextX][nextY] == 1) alive++;
            }

        }

        return alive;

    }

}
```

### APPROACH 2: In-place

To mark a currently alive cell as dead, we can change it to -1. In the method where we calculate the number of alive neighbours, we can take the absolute value of the cells. For a cell which is currently dead but it will be alive in the next round, we will have to use a different symbol. We cannot use 1 or -1 because it will affected our calculation of live neighbours.

```java
class Solution {
    
    public void gameOfLife(int[][] board) {

        int n = board.length;
        int m = board[0].length;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                
                int c = countAliveNeighbours(board, i, j);

                //to mark it as now dead, change it to -1
                //in the code to check for alive neighbours, we have updated to use absolute value
                if(board[i][j] == 1 && c < 2){
                    board[i][j] = -1;
                }

                //to mark it as now dead, change it to -1
                if(board[i][j] == 1 && c > 3){
                    board[i][j] = -1;
                }
                
                //to mark it as alive, use another symbol
                //if we use 1/-1, it will affect the number of live neighbours currently
                if(board[i][j] == 0 && c == 3){
                    board[i][j] = 2;
                }

            }
        }

        for(int i=0; i<n; i++){

            for(int j=0; j<m; j++){

                //we had used 2 to mark as alive in the next round
                if(board[i][j] == 2){
                    board[i][j] = 1;
                //we have used -1 to mark as dead in next round
                }else if(board[i][j] == -1){
                    board[i][j] = 0;
                }
            }
            
        }

    }

    public int countAliveNeighbours(int[][] board, int i, int j){

        int n = board.length;
        int m = board[0].length;

        int[] x = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] y = {-1, 0, 1, -1, 1, -1, 0, 1};

        int alive = 0;

        for(int k=0; k<8; k++){

            int nextX = i + x[k];
            int nextY = j + y[k];

            if(nextX >= 0 && nextX < n && nextY >=0 && nextY < m){
                if(Math.abs(board[nextX][nextY]) == 1) alive++;
            }

        }

        return alive;

    }

}
```
