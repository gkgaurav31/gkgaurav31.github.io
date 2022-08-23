---
layout: post
title: Minimum Area Rectangle
date: 2022-08-23 22:29 +0530
author: "Gaurav Kumar"
tags: "java arrays important"
categories: "arrays"
---

## Problem Description

We are given an array of [x,y] co-ordinates. Write a function which returns the minimum area of any rectangle that can be formed using any 4 points in this array. The sides of the rectangle must be parallel to the X and Y axis. If no rectangle can be formed, return 0.

### Solution

```java
import java.util.*;

class Program {

  public void convertPoints(int[][] p, HashSet<String> set){
    for(int i=0; i<p.length; i++){
      String point = p[i][0] + ":" + p[i][1];
      set.add(point);
    }
  }

  public String MyToString(int a, int b){
    return a + ":" + b;
  }
  
  public int minimumAreaRectangle(int[][] points) {

    int minArea = Integer.MAX_VALUE; //set to max value and check for min

    HashSet<String> set = new HashSet<>();
    convertPoints(points, set); //add all the [x,y] co-ordinates to the HashSet. We will use this later to look for required points. To make lookup simpler, we will convert [x,y] as a String "x:y"

    //We will pick two co-ordinates and check if they are diagnol. If they are not, then continue. 
    //If they are diagnol, we can figure out the other two co-ordinates which CAN potentially form a rectangle.
    //Check if the other two points exist in the HashSet we created earlier. If both are present, check if the area is min
    for(int i=0; i<points.length; i++){

      int x1 = points[i][0];     
      int y1 = points[i][1];

      for(int j=i; j<points.length; j++){

        int x2 = points[j][0];
        int y2 = points[j][1];

        //check if the two points are diagnol to each other
        boolean diagonalCheck = (x1 != x2) && (y1 != y2);

        //if both points lie on the same X or Y axis, then continue since we are looking for diagnol points.
        if(!diagonalCheck) continue;

        //The other co-ordinates will be:
        //[we don't care where they are placed in the 2D plane, but we just need to know whether they are present in the array]
        int x3 = x1;
        int y3 = y2;

        int x4 = x2;
        int y4 = y1;

        //if both points exist, calculate area and update min
        if(set.contains(MyToString(x3,y3)) && set.contains(MyToString(x4,y4))){
          //calculate area
          int area = Math.abs(x2-x1) * Math.abs(y2-y1);
          if(area < minArea){
            minArea = area;
          }
        }
        
      }
      
    }
    
    //If no diagnol points are found, return 0
    return (minArea == Integer.MAX_VALUE) ? 0 : minArea;
    
  }
}
```
