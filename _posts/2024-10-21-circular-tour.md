---
layout: post
title: Circular tour (geeksforgeeks - SDE Sheet)
date: 2024-10-21 21:31 +0530
author: "Gaurav Kumar"
tags: "java greedy geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Suppose there is a circle. There are N petrol pumps on that circle. You will be given two sets of data.

1. The amount of petrol that every petrol pump has.
2. Distance from that petrol pump to the next petrol pump.
   Find a starting point where the truck can start to get through the complete circle without exhausting its petrol in between.
   Note : Assume for 1 litre petrol, the truck can go 1 unit of distance.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/circular-tour-1587115620/1?page=7)

## SOLUTION

Similar Problem: [Gas Station]({% post_url 2023-01-29-gas-station %})

We first check if the total petrol from all pumps is at least equal to the total distance to the next pumps. If it is not, then it's impossible to complete the journey, and we return -1.

If it is possible, we then go through each pump and track the current petrol balance. If at any point the balance goes negative, we cannot start from that pump, so we set our starting point to the next pump and reset the balance.

The main things here is that when we encounter a petrol pump where the truck runs out of petrol, we don't need to check any previous starting positions. Instead, we can safely skip those pumps and start from the next one.

```java
class Solution {

    int tour(int petrol[], int distance[]) {

        int n = petrol.length;

        int totalPetrol = 0;
        int totalDistance = 0;

        // calculate the total petrol and total distance
        for (int i = 0; i < n; i++) {
            totalPetrol += petrol[i];
            totalDistance += distance[i];
        }

        // If total distance is greater than total petrol, it's impossible to complete the tour
        if (totalDistance > totalPetrol) {
            return -1;
        }

        // init:
        int idx = 0;
        int startPosition = 0;
        int currentPetrol = 0;

        // start from position idx and keep moving forward if possible
        while (idx < n) {

            // Update current petrol balance after moving to the next pump
            currentPetrol += petrol[idx] - distance[idx];

            // If current petrol balance goes negative
            // It's not possible to start from any previous position
            if (currentPetrol < 0) {
                startPosition = idx + 1;
                currentPetrol = 0;
            }

            // Move to the next pump
            idx++;

        }

        return startPosition;
    }
}
```
