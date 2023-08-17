---
layout: post
title: Snakes and Ladders
date: 2023-08-17 19:37 +0530
author: "Gaurav Kumar"
tags: "java graphs leetcode leetcode150 important"
categories: "graphs"
---
## PROBLEM DESCRIPTION

You are given an n x n integer matrix board where the cells are labeled from 1 to n2 in a Boustrophedon style starting from the bottom left of the board (i.e. board[n - 1][0]) and alternating direction each row.

You start on square 1 of the board. In each move, starting from square curr, do the following:

- Choose a destination square next with a label in the range [curr + 1, min(curr + 6, n2)].
- This choice simulates the result of a standard 6-sided die roll: i.e., there are always at most 6 destinations, regardless of the size of the board.
- If next has a snake or ladder, you must move to the destination of that snake or ladder. Otherwise, you move to next.
- The game ends when you reach the square n2.

A board square on row r and column c has a snake or ladder if board[r][c] != -1. The destination of that snake or ladder is board[r][c]. Squares 1 and n2 do not have a snake or ladder.

Note that you only take a snake or ladder at most once per move. If the destination to a snake or ladder is the start of another snake or ladder, you do not follow the subsequent snake or ladder.

For example, suppose the board is [[-1,4],[-1,3]], and on the first move, your destination square is 2. You follow the ladder to square 3, but do not follow the subsequent ladder to 4.

Return the least number of moves required to reach the square n2. If it is not possible to reach the square, return -1.

[leetcode](https://leetcode.com/problems/snakes-and-ladders/)

## SOLUTION

This is quite a tricky problem, but it has been well explained by here: [NeetCode](https://www.youtube.com/watch?v=6lH4nO3JfLk)

This problem is actually a graph problem. Since we are looking for the shortest distance and there are no weights involved, we can think of using BFS. We can start from the position number 1. In the board, the 1 actually starts from the bottom row. To make it simpler, we can reverse the rows of the board to use natural way of indexing. For a given position say idx, to get its index in the matrix, we can treate idx as idx-1 (basically, convert it to 0-index) while getting the index needed. The value in the board itself does not need to be converted. The other problem is that the even rows go in reverse order. So, when we find that the row is even, we will have to update teh column calculated based on reverse order.  

Coming to the actual BFS logic, we start by putting the first position and it's moves count (as a pair) to a queue. We keep remove elements from the queue and go to the next 6 places (because the dice can have maximum 6 value). It's possible that in the next position, we have a ladder or a snake. In that case, we can simply update the next position to wherever the ladder or snake will take us. We make this as visited and add it to the queue. If it happens to be the last position, we have actually got an answer since we have reached the destination. We can directly return is as it must be the lowest number of steps it took to reach the destination following breadth wise search.

```java
//class to store the position and how many moves it took to reach it from source
class Pair{

    int position;
    int moves;

    Pair(int p, int m){
        position = p;
        moves = m;
    }
}

class Solution {

    //reverse the rows of the board so that we start the counting from first row
    public int[][] reverseRows(int[][] board) {
        int numRows = board.length;
        int numCols = board[0].length;

        for (int i = 0; i < numRows / 2; i++) {
            int[] temp = board[i];
            board[i] = board[numRows - i - 1];
            board[numRows - i - 1] = temp;
        }
        return board;
    }


    public int snakesAndLadders(int[][] board) {

        int n = board.length;

        //reverse the board row wise only
        //this will make it easier for us to calculate the co-ordinated for a given "position" 1, 2, 3 ... n*n
        board = reverseRows(board);

        //for BFS
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(1, 0));

        //we don't want to visit the same position again
        //if we had already visited it earlier, it must have taken us lesser number of moves anyway
        Set<Integer> visited = new HashSet<>();

        //add the starting position
        visited.add(1);

        //BFS: while there are nothing more to explore
        while(!q.isEmpty()){
            
            //get current position
            Pair p = q.poll();
            int cPosition = p.position; //current position number
            int cMoves = p.moves;   //moves to reach current position

            //after rolling the dice, we can move to next 6 positions
            for(int i=1; i<=6; i++){
                
                //possible next position
                int nextPosition = cPosition + i;

                //index of next position
                int[] nextPositionIndex = getIndex(nextPosition, n);
                int nextPositionX =  nextPositionIndex[0];
                int nextPositionY =  nextPositionIndex[1];
                
                //if next position has a ladder or snake, use that (update nextPosition which we had calculated based on simple step)
                if(board[nextPositionX][nextPositionY] != -1){
                    nextPosition = board[nextPositionX][nextPositionY];
                }

                //if the next position is end of the square, we have got the answer
                if(nextPosition == (n*n))
                    return cMoves + 1;
                
                //otheriwse, add next position to visited (so that we don't visit it again)
                //also add it to the queue to check for possition path from the next position
                if(!visited.contains(nextPosition)){
                    visited.add(nextPosition);
                    q.add(new Pair(nextPosition, cMoves+1));
                }

            }

        }

        //if we never visited the destination and all positions have been explored, return -1
        return -1;
            
    }

    public int[] getIndex(int idx, int n){

        //if it was 0 indexed, the actual number would be 1 less
        idx = idx-1;

        int r = idx/n;
        int c = idx%n;

        if(r%2 == 1)
            c = n - c - 1;

        return new int[]{r, c};

    }

}
```
