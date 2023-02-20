---
layout: post
title: Detect Squares
date: 2023-02-20 20:28 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 mathematics"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are given a stream of points on the X-Y plane. Design an algorithm that:

- Adds new points from the stream into a data structure. Duplicate points are allowed and should be treated as different points.
- Given a query point, counts the number of ways to choose three points from the data structure such that the three points and the query point form an axis-aligned square with positive area.

An axis-aligned square is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the DetectSquares class:

- DetectSquares() Initializes the object with an empty data structure.
- void add(int[] point) Adds a new point point = [x, y] to the data structure.
- int count(int[] point) Counts the number of ways to form axis-aligned squares with point point = [x, y] as described above.

[leetcode](https://leetcode.com/problems/detect-squares/description/)

### Solution

```java
class DetectSquares {

    //store [x,y] points in HashMap and their count as value since duplicates are allowed
    HashMap<String, Integer> map;

    public DetectSquares() {
        map = new HashMap<>();    
    }
    
    //add point [x,y] to the HashMap
    public void add(int[] point) {
        String s = point[0] + ":" + point[1];
        map.put(s, map.getOrDefault(s, 0) + 1);
    }
    
    //get the count of squares which can be made for the given input point with 3 other points in the HashMap
    public int count(int[] point) {
        
        //init
        int count = 0;

        //loop through each point in the HashMap and check if it can be treated as a diagonal for the input point
        for(String k: map.keySet()){
            
            int x = Integer.parseInt(k.split(":")[0]);
            int y = Integer.parseInt(k.split(":")[1]);

            //if the current co-ordinate's x or y is same as input point, it cannot be a diagnonal so we can continue            
            if(x == point[0] || y == point[1])
                continue;
            
            //if [x,y] can be a diagnonal for the input point, we can determine the co-ordinates of other two points 
            //but at this point, it can be a rectangle also. We will add a check for square also.
            //First Point: [x1, y1] = [p[0], y]
            //Second Point: [x2, y2] = [x, p[1]]
            int x1 = point[0];
            int y1 = y;
            int x2 = x;
            int y2 = point[1];

            //if the above two points are present in the HashMap, then we can check whether they form a square
            //if length of both sides which are parallel to X and Y are same, they must be a square
            if(map.containsKey((x1 + ":" + y1)) && map.containsKey((x2 +":" +y2)) && Math.abs(x1-x2) == Math.abs(y1-y2)){

                    //since duplicates are allowed, the number of ways we can form that square is:
                    //count += frequency of point 1 + frequency of point 2 + frequency of current diagonal point
                    count +=( map.get(x1 + ":" + y1) * map.get(x2 +":" +y2) * map.get(k));
            }

        }

        return count;

    }
}

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares obj = new DetectSquares();
 * obj.add(point);
 * int param_2 = obj.count(point);
 */
```
