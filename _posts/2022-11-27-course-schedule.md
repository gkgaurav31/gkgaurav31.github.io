---
layout: post
title: Course Schedule
date: 2022-11-27 14:22 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.  

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.  

Return true if you can finish all courses. Otherwise, return false.

[leetcode](https://leetcode.com/problems/course-schedule/description/)

### Solution

If there is a loop in the graph, we can return false. We can also use Topological Sort to solve this problem.

![snapshot]({{ site.baseurl }}/assets/img/course_schedule.png)

```java
class Solution {
    
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        
        //create adjacency list using prerequisites
        ArrayList<Integer>[] adj = new ArrayList[numCourses];
        for(int i=0; i<numCourses; i++){
            adj[i] = new ArrayList<Integer>();
        }
        for(int i=0; i<prerequisites.length; i++){
            adj[prerequisites[i][0]].add(prerequisites[i][1]);
        }

        //in-degree (number of edges coming in)
        //in this problem, it will tell us how many cources depend on the current course
        //if in-degree is 0, no course is dependent on the current course.
        int[] indegree = new int[numCourses];
        for(int i=0; i<prerequisites.length; i++){
            indegree[prerequisites[i][1]]++;
        }

        //add nodes with in-degree 0 to a queue (those courses on which no other course depends on)
        Queue<Integer> q = new LinkedList<>();
        for(int i=0; i<numCourses; i++){
            if(indegree[i] == 0){
                q.add(i);
            }
        }

        int processed = 0;

        while(!q.isEmpty()){

            //Take out the course with 0 indegree from the queue
            Integer x = q.poll();

            //increase the processed count
            processed++;

            //get the list of neighbours for that course
            List<Integer> neighbours = adj[x];

            //removing this node x means that indegree of neighbours of x will decrease by 1
            for(int i=0; i<neighbours.size(); i++){
                
                indegree[neighbours.get(i)]--;

                //If the indegree becomes 0 after decreasing it, we can add that to the queue
                if(indegree[neighbours.get(i)] == 0){
                    q.add(neighbours.get(i));
                }

            }

        }

        //finally, the number of processed courses should be equals to numcourses if all of them can  be completed
        return processed == numCourses;

    }
}
```
