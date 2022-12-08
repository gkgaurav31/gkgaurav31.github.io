---
layout: post
title: K Closest Points to Origin
date: 2022-12-06 23:46 +0530
author: "Gaurav Kumar"
tags: "java heap leetcode neetcode150" 
categories: "neetcode150"
---

## Problem Description

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).  

[leetcode](https://leetcode.com/problems/k-closest-points-to-origin/description/)

### Solution

```java
class Solution {

    public double getDistanceFromOrigin(double x, double y){
        return Math.sqrt(x*x + y*y);
    }

    public int[][] kClosest(int[][] points, int k) {
        
        PriorityQueue<Pair> pq = new PriorityQueue<>();

        for(int i=0; i<points.length; i++){
            
            int x = points[i][0];
            int y = points[i][1];

            double dist = getDistanceFromOrigin(x, y);

            pq.add(new Pair(dist, new int[]{x,y})); 
            
        }

        int[][] ans = new int[k][2];

        for(int i=0; i<k; i++){
            Pair p = pq.poll();

            int x = p.coordinates[0];
            int y = p.coordinates[1];
            
            ans[i] = new int[]{x,y};
        }

        return ans;

    }
}

class Pair implements Comparable<Pair>{

    double distance;
    int[] coordinates;

    Pair(double d, int[] c){
        this.distance = d;
        this.coordinates = c;
    }

    public int compareTo(Pair p){
        if(this.distance < p.distance) return -1;
        if(this.distance > p.distance) return 1;
        return 0;
    }

}
```

### Another way to code

```java
class Solution {

    public int distance(int x, int y){
        return x*x + y*y; //we don't necessarily need square root here since all we need to do is sort. Math.sqrt() will also work.
    }

    public int[][] kClosest(int[][] points, int k) {
        
        //Compare two arrays based on their first element at 0th index.
        PriorityQueue<Integer[]> pq = new PriorityQueue<>((a,b) -> a[0]-b[0]);

        for(int i=0; i<points.length; i++){

            Integer x = points[i][0];
            Integer y = points[i][1];
            
            pq.add(new Integer[]{distance(x,y), x, y});

        }

        int[][] ans = new int[k][2];

        for(int i=0; i<k; i++){

            Integer[] p = pq.poll();

            ans[i] = new int[]{p[1], p[2]};
        }

        return ans;

    }

}
```
