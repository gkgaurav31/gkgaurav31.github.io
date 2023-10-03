---
layout: post
title: Candy Crush
date: 2023-10-03 20:47 +0530
author: "Gaurav Kumar"
tags: "java matrix leetcode leetcodealgo100"
categories: "matrix"
---

## PROBLEM DESCRIPTION

Given an m x n integer array `board` representing the grid of candy where `board[i][j]` represents the type of candy. A value of `board[i][j] == 0` represents that the cell is empty.

The given board represents the state of the game following the player's move. Now, you need to restore the board to a stable state by crushing candies according to the following rules:

1. If three or more candies of the same type are adjacent vertically or horizontally, crush them all at the same time - these positions become empty.
2. After crushing all candies simultaneously, if an empty space on the board has candies on top of itself, then these candies will drop until they hit a candy or bottom at the same time. No new candies will drop outside the top boundary.
3. After the above steps, there may exist more candies that can be crushed. If so, you need to repeat the above steps.
4. If there does not exist more candies that can be crushed (i.e., the board is stable), then return the current board.

You need to perform the above rules until the board becomes stable, then return the stable board.

[leetcode](https://leetcode.com/problems/candy-crush/)

## SOLUTION

- First we mark all the positions which should be removed in the current matrix
  - We can do this row wise first. Whenever we see at least three elements with same value, mark them. We can change the value to -ve to mark them.
  - Do the same thing while traversing column wise
- Once all of them are marked, go to each column and start filling up the position from bottom to top. We can use some pointer pos to start from bottom most position. Use another variable starting from bottom to top to get the next element which is greater than 0 (which means that it is not supposed to be removed). When we get such an element, put it in position pos and move pos one cell above it. Once all possible values are filled, mark rest of the elements from [pos,0] as 0.
- After this, the same steps need to be repeated. If we did not find any triplet which can be removed from the matrix, we have reached a stable state. In that case we can simply return the board. We can use a boolean variable to check if there was at least one triplet which could be removed.
- If the state is not stable, which means that there is at least one triplet which can be removed, then we can recursively call our main method as this will act like a sub-problem.

```java
class Solution {

    public int[][] candyCrush(int[][] board) {

        int n = board.length; // Number of rows in the board
        int m = board[0].length; // Number of columns in the board

        boolean stable = true; // A flag to determine if the board is stable after a pass

        // Mark and crush candy in rows
        for(int r=0; r<n; r++){

            //window for the triplet
            int left=0;
            int right=2;

            //while there are more rows left
            while(right<m){

                int x = Math.abs(board[r][left]); // Get the absolute value of the candy at the leftmost position in the window

                if(x == 0){
                    left++; right++; continue; // If it's empty, move the window
                }

                boolean matchFound = true;

                // Check if all candies in the window have the same type
                for(int c=left+1; c<=right; c++){
                    if(Math.abs(board[r][c]) != x){
                        matchFound = false;
                    }
                }

                //we found a triplet which should be removed
                if(matchFound){

                    stable = false; // We found a match, so the board is not stable

                    // Crush the candies in the window by marking them as negative values
                    for(int c=left; c<=right; c++){
                        board[r][c] = -x;
                    }

                }

                left++; right++;

            }

        }

        // Mark and crush candy in columns
        for(int c=0; c<m; c++){

            // vertical window for the triplet
            int top=0;
            int bottom=2;

            // while we have more columns
            while(bottom<n){

                int x = Math.abs(board[top][c]); // Get the absolute value of the candy at the topmost position in the window

                if(x == 0){
                    top++; bottom++; continue; // If it's empty, move the window
                }

                boolean matchFound = true;

                // Check if all candies in the window have the same type
                for(int r=top+1; r<=bottom; r++){
                    if(Math.abs(board[r][c]) != x){
                        matchFound = false;
                    }
                }

                // found a triplet which should be removed
                if(matchFound){

                    stable = false; // We found a match, so the board is not stable

                    // Crush the candies in the window by marking them as negative values
                    for(int r=top; r<=bottom; r++){
                        board[r][c] = -x;
                    }

                }

                top++; bottom++;

            }

        }

        if(stable){
            return board; // If the board is stable, return the current state
        }

        rearrange(board); // Otherwise, rearrange the board to drop candies

        return candyCrush(board); // Recursively repeat the process until the board becomes stable

    }

    // Rearrange the board to drop candies
    public void rearrange(int[][] board){

        int n = board.length;
        int m = board[0].length;

        for(int c=0; c<m; c++){

            int pos = n-1;

            for(int r=n-1; r>=0; r--){

                if(board[r][c] > 0){
                    board[pos][c] = board[r][c]; // Move candies down to needed position
                    pos--; // move to next position where the next valid value can be added
                }

            }

            // Mark the rest of the column as empty (0)
            for(int r=pos; r>=0; r--){
                board[r][c] = 0;
            }

        }

    }

}
```
