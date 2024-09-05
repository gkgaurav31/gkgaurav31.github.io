---
layout: post
title: Tower Of Hanoi (geeksforgeeks - SDE Sheet)
date: 2024-09-05 21:54 +0530
author: "Gaurav Kumar"
tags: "java recursion geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The tower of Hanoi is a famous puzzle where we have three rods and n disks. The objective of the puzzle is to move the entire stack to another rod. You are given the number of discs n. Initially, these discs are in the rod 1. You need to print all the steps of discs movement so that all the discs reach the 3rd rod. Also, you need to find the total moves.

You only need to complete the function toh() that takes following parameters: n (number of discs), from (The rod number from which we move the disc), to (The rod number to which we move the disc), aux (The rod that is used as an auxiliary rod) and prints the required moves inside function body (See the example for the format of the output) as well as return the count of total moves made.

Note: The discs are arranged such that the top disc is numbered 1 and the bottom-most disc is numbered n. Also, all the discs have different sizes and a bigger disc cannot be put on the top of a smaller disc. Refer the provided link to get a better clarity about the puzzle.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/tower-of-hanoi-1587115621/1?page=5)

## SOLUTION

[Good Explanation on YouTube by Abdul Bari](https://www.youtube.com/watch?v=q6RicK1FCUs)

```java
class Hanoi {

    public long toh(int n, int from, int to, int aux) {

        if(n == 0)
            return 0;

        int moves = 0;

        moves += toh(n-1, from, aux, to);

        System.out.println("move disk " + n + " from rod " + from + " to rod " + to);

        moves++;

        moves += toh(n-1, aux, to, from);

        return moves;

    }
}
```
