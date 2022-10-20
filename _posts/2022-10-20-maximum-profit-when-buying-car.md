---
layout: post
title: Maximum Profit When Buying a Car
date: 2022-10-20 23:22 +0530
author: "Gaurav Kumar"
tags: "java heap greedy important"
categories: "greedy"
---

## Problem Description

Given two arrays, A and B of size N. A[i] represents the time by which you can buy the ith car without paying any money.  
B[i] represents the profit you can earn by buying the ith car. It takes 1 minute to buy a car, so you can only buy the ith car when the current time <= A[i] - 1.  
Your task is to find the maximum profit one can earn by buying cars considering that you can only buy one car at a time.  

### Solution

- Sort by time of buying in ascending order (need to use Pairs, with comparator)
- Iterate from left to right and maintain a min heap to get the min profit which is possible till current index. If the size of min heap is less than the current time when we can buy the add, simply add the profit associated. But, if it's >= the time, then we need to take a decision. If the min possible profit till not is less than the profit for current element we are looking at, then we should add this profit. We can easily get the min possible profit till now using min heap. We already know the profit we would get if we purchase the current car. So just take the maximum between and discard the other one. Keep doing this till end of the list.
- Finally to calculate the total profit, maintain a total profit variable and keep adding to it while removing min element from the min heap.

```java
public class Solution {

    public static final int M = 1000000007;

    public int solve(ArrayList<Integer> A, ArrayList<Integer> B) {

        int n = A.size();

        List<Pair> list = new ArrayList<>();
        for(int i=0; i<n; i++){
            list.add(new Pair(A.get(i), B.get(i)));
        }

        Collections.sort(list);

        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for(int i=0; i<n; i++){

            Pair p = list.get(i);

            int time = p.time;
            int profit = p.profit;

            if(pq.size() < time){
                pq.add(p.profit);
            }else if(pq.peek() < profit){
                pq.poll();
                pq.add(profit);
            }
        }

        int total=0;

        while(pq.size() != 0) total = (total%M + pq.poll()%M)%M;

        return total;

    }

}

class Pair implements Comparable<Pair>{

    int time;
    int profit;
    
    Pair(int s, int e){
        time = s;
        profit = e;
    }

    @Override
    public int compareTo(Pair a){
        if(a.time < this.time) return 1;
        if(a.time == this.time) return 0;
        return -1;
    }

    @Override
    public String toString(){
        return "Time: " + this.time + " Profit: " + this.profit;
    }

}
```
