---
layout: post
title: Word Search II
date: 2023-09-16 00:11 +0530
author: "Gaurav Kumar"
tags: "java leetcode leetcode150 recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given an m x n board of characters and a list of strings words, return all words on the board.
Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

[leetcode](https://leetcode.com/problems/word-search-ii)

## Solution

This solution uses a Trie data structure to efficiently search for words on a given board of characters. It starts by building a Trie containing all the words we want to find on the board. Then, it iterates through each cell of the board and checks if the current character can be the beginning of a word in the Trie. If it can, it performs a depth-first search (DFS) starting from that cell, checking neighboring cells for the next character in the Trie. If the search reaches the end of a word in the Trie, it adds that word to the result list, ensuring no duplicates. The algorithm uses backtracking to explore different paths and marks visited cells to avoid revisiting them.

- Add all the words in the dictionary to a trie. This will help in efficient lookup.
- Loop through each cell on the board
- If the character

```java
class Solution {

    public List<String> findWords(char[][] board, String[] words) {

        int n = board.length;
        int m = board[0].length;

        // Create a Trie data structure to efficiently store and search for words
        Trie trie = new Trie();
        TrieNode root = trie.root;

        // Insert all words from the word list into the Trie
        for (String word : words){
            trie.insertWord(word);
        }

        List<String> result = new ArrayList<>();

        // Explore each cell of the board (using DFS)
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                dfs(board, i, j, result, root);
            }
        }

        return result;

    }

    // Depth-First Search to explore possible words on the board
    public void dfs(char[][] board, int i, int j, List<String> result, TrieNode root){

        // Return if the current cell is out of bounds
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length)
            return;

        // If the current cell is already marked as visited or the character is not in the Trie, return
        if (board[i][j] == '$' || root.children[board[i][j] - 'a'] == null)
            return;

        // Move to the Trie node corresponding to the current character
        root = root.children[board[i][j] - 'a'];

        // If the current Trie node represents the end of a word, add it to the result
        if (root.isEnd == true){
            result.add(root.word);
            root.isEnd = false; // To avoid duplicates
        }

        // Define directions for exploring neighboring cells
        int[] x = {0, 0, 1, -1};
        int[] y = {1, -1, 0, 0};

        // Backup the current character and mark it as visited
        char temp = board[i][j];
        board[i][j] = '$'; // Mark visited

        // Explore neighboring cells recursively
        for (int k = 0; k < 4; k++){

            int nextX = i + x[k];
            int nextY = j + y[k];

            dfs(board, nextX, nextY, result, root);

        }

        // Backtrack by restoring the original character
        board[i][j] = temp; // Backtrack

    }

}

class TrieNode {

    TrieNode[] children;
    String word;
    boolean isEnd;

    TrieNode(){
        children = new TrieNode[26];
        word = "";
        isEnd = false;
    }

}

class Trie {

    TrieNode root;

    Trie(){
        root = new TrieNode();
    }

    // Insert a word into the Trie
    void insertWord(String word){

        TrieNode r = root;

        for (int i = 0; i < word.length(); i++){

            char ch = word.charAt(i);

            if (r.children[ch - 'a'] == null){
                r.children[ch - 'a'] = new TrieNode();
            }

            r = r.children[ch - 'a'];

        }

        r.isEnd = true;
        r.word = word;

    }

}
```
