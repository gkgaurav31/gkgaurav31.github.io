---
layout: post
title: Word Search
date: 2023-01-06 22:58 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking neetcode150 important"
categories: "neetcode150"
---

## Problem Description

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

[leetcode](https://leetcode.com/problems/word-search/)

### Solution

At each cell, check all possible paths and form a word. Check if it matches the given word.

### BRUTE-FORCE (TLE)

```java
class Solution {

    public boolean exist(char[][] board, String word) {

        for(int i=0; i<board.length; i++){
            for(int j=0; j<board[0].length; j++){
                if(existHelper(board, word, new StringBuilder(""), i, j)) return true;
            }
        }

        return false;
    }

    public boolean existHelper(char[][] board, String word, StringBuilder current, int i, int j){

        if(current.toString().equals(word)) return true;

        if(i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;

        if(board[i][j] != '$'){

            char c = board[i][j];
            current.append(c);

            board[i][j] = '$';
            
            boolean up = existHelper(board, word, current, i-1, j);
            boolean left = existHelper(board, word, current, i, j-1);
            boolean down = existHelper(board, word, current, i+1, j);
            boolean right = existHelper(board, word, current, i, j+1);

            board[i][j] = c;
            current.deleteCharAt(current.length() - 1);

            return up || left || down || right;
            
        }

        return false;

    }

}
```

### OPTIMIZED

Instead of constructing the "current" string, we can keep a track of count of matching characters. If that becomes equal to the characters present in the orignal array, we have found a match.

```java
class Solution {
    
    public boolean exist(char[][] board, String word) {
        
        int n = board.length;
        int m = board[0].length;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                boolean present = existHelper(board, word, 0, i, j);
                if(present) return present;
            }
        }

        return false;

    }

    public boolean existHelper(char[][] board, String word, int idx, int row, int column){

        //idx represents the position till which the characters have matched
        //if idx is equal to lenght of given word, it means that all the characters must have matched before it
        //so we can return true
        if(idx >= word.length()) return true;

        //boundary condition
        //if the current character in the board does not match the character at idx of the word, we can return false as there is no chance it can match later
        if(row < 0 || row >= board.length || column < 0 || column >= board[0].length || board[row][column] != word.charAt(idx)) return false;

        //if the current char has been marked as #, which means that it was visited before
        if(board[row][column] != '#'){
            
            //store the current character so that we can restore it while backtracking -> IMPORTANT STEP
            char c = board[row][column];
            //set the current char in the board to # so that we don't visit it again
            board[row][column] = '#';
            
            //go to all other directions
            boolean top = existHelper(board, word, idx+1, row-1, column);
            boolean left = existHelper(board, word, idx+1, row, column-1);
            boolean down = existHelper(board, word, idx+1, row+1, column);
            boolean right = existHelper(board, word, idx+1, row, column+1);

            //backtrack
            board[row][column] = c;

            //if any of the options found a match we can return true
            return top || left || down || right;

        }else{
            //if it's the next character and it's already visited, we can return false
            return false;
        }

    }

}
```

### ANOTHER WAY TO CODE

```java
class Solution {

    public boolean exist(char[][] board, String word) {
        
        int n = board.length;
        int m = board[0].length;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                boolean present = existHelper(board, word, 0, i, j);
                if(present) return present;
            }
        }

        return false;

    }

    public boolean existHelper(char[][] board, String word, int idx, int row, int column){

        if(idx >= word.length()) return true;

        if(row < 0 || row >= board.length || column < 0 || column >= board[0].length || board[row][column] != word.charAt(idx)) return false;

        if(board[row][column] != '#'){

            char c = board[row][column];
            board[row][column] = '#';

            //direction helper
            int[] x = {-1, 0, 1, 0};
            int[] y ={0, -1, 0, 1};

            for(int k=0; k<4; k++){
                boolean found = existHelper(board, word, idx+1, row+x[k], column+y[k]);
                if(found) return found;
            }

            board[row][column] = c;

        }
        
        return false;
        
    }

}
```
