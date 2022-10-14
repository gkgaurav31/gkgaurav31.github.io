---
layout: post
title: Zigzag Traversal
date: 2022-10-15 02:21 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"
---

## Problem Description

Given an m x n matrix mat, return an array of all the elements of the array in a zigzag/diagonal order.  

Input: mat = [[1,2,3],[4,5,6],[7,8,9]]  
Output: [1,2,4,7,5,3,6,8,9]  

[leetcode](https://leetcode.com/problems/diagonal-traverse/)

### Solution

```java
import java.util.*;

class Program {
  public static List<Integer> zigzagTraverse(List<List<Integer>> array) {

    //Answer list
    List<Integer> output = new ArrayList<>();

    //Height of the matrix
    int height = array.size() - 1;

    //Width of the matrix
    int width = array.get(0).size() - 1;

    //We will be starting by doing downwards from the first element
    boolean goingDown = true;

    //Initialize the row, col to 0 to start with
    int row = 0;
    int col = 0;

    //Check if the current row, col is within the bound of height and width of the matrix
    while(isInbound(row, col, height, width)){

      //"visit" the current element
      output.add(array.get(row).get(col));

      //Going Down
      if(goingDown){
        //If we are at the last row or first column, we cannot go diagonally down next
        if(row == height || col == 0){
          //So we change the direction
          goingDown = false;

          //If we are at the last row, we need to go right
          if(row == height){
            col++;
          //If we are not at the last row (but the first column), we can go down
          }else{
            row++;
          }
        //Else, just go diagonally downwards
        }else{
          col--;
          row++;
        }

      //Going Up
      }else{
        //If we are at the top most row or right most column, we cannot go diagonally upwards next
        if(row == 0 || col == width){
          //So, we change the direction
          goingDown = true;

          //If we are at the right most column, we need to go downwards
          if(col == width){
            row++;
          //If we are not at the right most column, but we are at the top most row, we can go right
          }else{
            col++;
          }
        //Else, just go diagonally upwards
        }else{
          row--;
          col++;
        }
        
      }
      
    }  

    return output;
    
  }

  public static boolean isInbound(int row, int col, int h, int w){
    if(row < 0 || row > h || col < 0 || col > w) return false;
    return true;
  }


  
}
```
