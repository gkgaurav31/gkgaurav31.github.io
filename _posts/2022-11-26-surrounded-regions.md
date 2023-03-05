---
layout: post
title: Surrounded Regions
date: 2022-11-26 21:47 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

Given an m x n matrix board containing 'X' and 'O', capture all regions that are 4-directionally surrounded by 'X'.  

A region is captured by flipping all 'O's into 'X's in that surrounded region.  

[leetcode](https://leetcode.com/problems/surrounded-regions/description/)

### Solution

The key observation in this problem is that if there is any 'O' in the bordering row or columns, it will impact all connected cells. (Since the 'O' at the border cannot be flipped to 'X' as it's cannot be surrounded by water on all 4 sides). Which means, it will impact all its neighboring cells as well, if they are not already 'X'. This will continue like a "chain reaction". So, what we can is, for each bordering cell, use BFS/DFS and mark all connected/impacted cells. We are setting those cells with '#' to avoid visiting them again, which is possible from some other cell.

Main steps:

- Iterated over the boundard and find O's
- Every time we find an O, perform DFS from that position
- In DFS convert all 'O' to '#' (So that we can differentiate which 'O' can be flipped and which cannot be)
- After all DFSs have been performed, board contains three elements,#, O and X
- 'O' are left over elements which are not connected to any boundary O, so flip them to 'X'
- '#' are elements which cannot be flipped to 'X', so flip them back to 'O'

```java
class Solution {
    
    public void solve(char[][] board) {

        int n = board.length; int m = board[0].length;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                
                //Check if the current position is at the border
                if(i == 0 || j == 0 || i == n-1 || j == m-1){
                    
                    //If the bordering cell is X, continue
                    if(board[i][j] == 'X') continue;
                    //else use DFS to find connected cells
                    dfs(board, i, j);
                }

            }
        }

        //At this point, the cells which cannot be flipped will be marked as #
        //So, all we need to do it change # to O. Other cells can be flipped, so mark them as X.
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){

                //If the cell is marked as #, it's connected to one of the bordering cells
                if(board[i][j] == '#'){
                    board[i][j] = 'O';
                //If not, we can flip it to X
                }else{
                    board[i][j] = 'X';
                }
            }
        }

    }

    public void dfs(char[][] board, int i, int j){
        
        //If the current cells is # -> alraedy marked or X -> no action needed, we can return
        if(i<0 || j<0 || i>=board.length || j>=board[0].length || board[i][j] == '#' || board[i][j] == 'X') return;

        //direction helper
        int[] x = {0,0,1,-1};
        int[] y = {1,-1,0,0};

        //mark the current cell visited
        board[i][j] = '#';

        //go to each direction and check if it's 'O'
        //in that case, we will mark is with #
        for(int k=0; k<4; k++){
            dfs(board, i+x[k], j+y[k]);
        }
        
    }

}
```
