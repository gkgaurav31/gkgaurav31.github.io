---
layout: post
title: Snake and Ladder Problem (geeksforgeeks - SDE Sheet)
date: 2024-10-01 22:03 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a 5x6 snakes and ladders board, find the minimum number of dice throws required to reach the destination or last cell (30th cell) from the source (1st cell).

You are given an integer N denoting the total number of snakes and ladders and an array arr[] of 2*N size where 2*i and (2\*i + 1)th values denote the starting and ending point respectively of ith snake or ladder. The board looks like the following.
Note: Assume that you have complete control over the 6 sided dice. No ladder starts from 1st cell.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/snake-and-ladder-problem4816/1?page=9)

## SOLUTION

We can start this by representing the Snakes and Ladders board as an array where each index corresponds to a cell on the board.

If there is a snake or ladder at a particular cell, the array stores the destination cell for that snake or ladder.

We then use a breadth-first search (BFS) approach to explore all possible moves from the starting position (cell 1), where each move corresponds to a dice roll (1 to 6).

For each move, we check if it leads to a snake or ladder and adjust the position accordingly. A queue is used to keep track of the current position and the number of dice throws taken to reach it.

We mark each visited cell to avoid revisiting it, and as soon as we reach the last cell (30), we return the number of dice throws taken.

```java
class Solution {

    // The final destination of the given board is 30
    private static final int LAST_POSITION = 30;

    static int minThrow(int n, int arr[]) {

        // Board represents the game board, size 31 because positions are from 1 to 30
        // Initialize all positions to -1 (no ladder or snake by default)
        int[] board = new int[31];

        // -1 means no snake or ladder on that cell
        Arrays.fill(board, -1);

        // Fill the board with ladders and snakes
        // The array arr[] contains pairs of start and end points of ladders and snakes
        for(int i = 0; i < n * 2; i += 2) {
            // arr[i] is the start point of a ladder or snake
            // arr[i + 1] is the endpoint
            // so set board[start] = end
            board[arr[i]] = arr[i + 1];
        }

        // To track which cells have been visited already
        boolean[] visited = new boolean[31];

        // A queue to implement the Breadth-First Search (BFS) algorithm
        // We start from cell 1 with a distance of 0
        Queue<Cell> q = new LinkedList<>();

        // Mark the first cell as visited
        visited[1] = true;

        // Add the first cell (1, 0 throws) to the queue
        q.add(new Cell(1, 0));

        // BFS loop
        while(!q.isEmpty()) {

            Cell currentCell = q.poll();

            int currentPosition = currentCell.position;
            int currentDistance = currentCell.distance;

            // Roll the dice (1 to 6)
            for(int dice = 1; dice <= 6; dice++) {

                int nextPosition = currentPosition + dice;

                // If we reach the last position (cell 30), return the number of throws
                if (nextPosition == LAST_POSITION)
                    return 1 + currentDistance;

                // If the next position has a snake or ladder, move to the destination
                if (board[nextPosition] != -1)
                    nextPosition = board[nextPosition];

                // If the next position has not been visited yet
                if (!visited[nextPosition]) {

                    // Mark it as visited
                    visited[nextPosition] = true;

                    // Add this new position to the queue with the updated distance
                    q.add(new Cell(nextPosition, currentDistance + 1));

                }

            }

        }

        // not possible to reach the destination
        return -1;

    }

}

// represents cell of the board
class Cell {

    int position;
    int distance;

    public Cell(int position, int distance) {
        this.position = position;
        this.distance = distance;
    }

}
```
