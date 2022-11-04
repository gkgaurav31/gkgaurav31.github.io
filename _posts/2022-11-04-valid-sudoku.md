---
layout: post
title: Valid Sudoku
date: 2022-11-04 19:18 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

- Each row must contain the digits 1-9 without repetition.
- Each column must contain the digits 1-9 without repetition.
- Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

Note:

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

[leetcode](https://leetcode.com/problems/valid-sudoku/description/)

### Solution

Checking the rows and columns is straight forward. For each row and each column, we can use a temp hashmap and check for duplicates.  

For each 3x3 square, we need a way to determine which cell belongs to which big box. An easy way to do this is by dividing the row and column index by 3 (There are 3 big boxed in each row and column). This will be an integer division. Example: a[8,8] -> big[8/3,8/3] = big[2,2].

[Detailed Explanation - YouTube - NeetCode](https://www.youtube.com/watch?v=TjFXEUCMqI8)

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        return checkRows(board) && checkColumns(board) && checkBigSquares(board);
    }

    public boolean checkBigSquares(char[][] b){

        Map<String, Set<Character>> map = new HashMap<>(); //Key: RowColumn as string

        for(int i=0; i<b.length; i++){
            for(int j=0; j<b.length; j++){

                String rc = (i/3)+""+(j/3);
                Character c = b[i][j];

                if(c == '.') continue;

                if(map.containsKey(rc)){
                    Set<Character> set = map.get(rc);
                    if(set.contains(c)) return false;
                    else set.add(c);
                }else{
                    Set<Character> set = new HashSet<>();
                    set.add(c);
                    map.put(rc, set);
                }

            }
        }

        return true;

    }

    public boolean checkColumns(char[][] b){

        Set<Character> set = new HashSet<>();

        for(int i=0; i<b[0].length; i++){
            set = new HashSet<>();
            for(int j=0; j<b.length; j++){
                
                Character c = b[j][i];

                if(c == '.') continue;

                if(set.contains(c)) return false;
                else set.add(c);
            }
        }

        return true;

    }

    public boolean checkRows(char[][] b){

        Set<Character> set = new HashSet<>();

        for(int i=0; i<b.length; i++){
            set = new HashSet<>();
            for(int j=0; j<b[0].length; j++){
                Character c = b[i][j];
                if(c == '.') continue;

                if(set.contains(c)){
                    return false;
                }else{
                    set.add(c);
                }
            }
        }

        return true;

    }

}
```
