---
layout: post
title: Word Boggle (geeksforgeeks - SDE Sheet)
date: 2024-09-07 22:44 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a dictionary of distinct words and an M x N board where every cell has one character. Find all possible words from the dictionary that can be formed by a sequence of adjacent characters on the board. We can move to any of 8 adjacent characters

Note: While forming a word we can move to any of the 8 adjacent cells. A cell can be used only once in one word.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/word-boggle4143/1?page=5)

## SOLUTION

### APPROACH 1 (TLE)

Traverse through each cell and apply dfs. Forming words during DFS and check if that word is present in the dictionary. If so, add it to the answer list.

```java
class Solution
{
    public String[] wordBoggle(char board[][], String[] dictionary)
    {

        Set<String> set = new HashSet<>();
        for(String s: dictionary)
            set.add(s);


        int n = board.length;
        int m = board[0].length;

        Set<String> list = new HashSet<>();

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                dfs(board, set, i, j, new StringBuffer(), list);
            }
        }


        String[] res = new String[list.size()];

        int i=0;
        for(String s: list){
            res[i++] = s;
        }

        return res;

    }

    public void dfs(char[][] board, Set<String> set, int i, int j, StringBuffer sb, Set<String> list){

        if(i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] == '#')
            return;


        sb.append(board[i][j]);

        char temp = board[i][j];
```

### APPROACH 2 (ACCEPTED)

A better way is to iterate throught the list of words in the dictionary and try to check if that word is present in the board.

```java
class Solution
{
    public String[] wordBoggle(char board[][], String[] dictionary)
    {

        int n = board.length;
        int m = board[0].length;

        List<String> list = new ArrayList<>();

        for(int k=0; k<dictionary.length; k++){

            for(int i=0; i<n; i++){

                boolean found = false;

                for(int j=0; j<m; j++){

                    // break to avoid repetition
                    if(dfs(board, dictionary[k], i, j, 0)){
                        list.add(dictionary[k]);
                        found = true;
                        break;
                    }

                }

                // if the current word was found, no need to check further, break;
                if(found) break;

            }

        }

        String[] res = new String[list.size()];
        for(int i=0; i<list.size(); i++){
            res[i] = list.get(i);
        }

        return res;

    }

    public boolean dfs(char board[][], String word, int positionX, int positionY, int idx){

        int n = board.length;
        int m = board[0].length;

        int i = positionX;
        int j = positionY;

        if(idx == word.length()){
            return true;
        }

        if(i < 0 || j < 0 || i >= n || j >= m || board[i][j] == '#')
            return false;

        if(board[i][j] != word.charAt(idx))
            return false;

        char temp = board[i][j];
        board[i][j] = '#';

        boolean found = false;

        int[] x = {0, 0, 1, 1, 1, -1, -1, -1};
        int[] y = {1, -1, 1, 0, -1, 1, 0, -1};

        for(int k=0; k<8; k++){

            int nextX = i + x[k];
            int nextY = j + y[k];

            if(dfs(board, word, nextX, nextY, idx+1)){
                found |= true;
            }

        }

        board[i][j] = temp;

        return found;

    }



}
```
