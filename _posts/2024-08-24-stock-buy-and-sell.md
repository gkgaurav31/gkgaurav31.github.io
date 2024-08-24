---
layout: post
title: Stock buy and sell (geeksforgeeks - SDE Sheet)
date: 2024-08-24 21:35 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The cost of stock on each day is given in an array A[] of size N. Find all the segments of days on which you buy and sell the stock such that the sum of difference between sell and buy prices is maximized. Each segment consists of indexes of two elements, first is index of day on which you buy stock and second is index of day on which you sell stock.
Note: Since there can be multiple solutions, the driver code will print 1 if your answer is correct, otherwise, it will return 0. In case there's no profit the driver code will print the string "No Profit" for a correct solution.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/stock-buy-and-sell-1587115621/1?page=1)

## SOLUTION

We set the buy and sell pointer to first stock and try to see if we can sell it. Keep moving the sell pointer forward if the price is higher. If it is lesser, then we have potentially got a segment - if sell pointer > buy pointer (otherwise we would just be trying to sell the same stock on same day with no profit).

It's possible that for the last stock, we keep going towards right to check if we can sell it and the loop ends at the last index. We can handle that case separately.

```java
class Solution{

    //Function to find the days of buying and selling stock for max profit.
    ArrayList<ArrayList<Integer> > stockBuySell(int A[], int n) {

        ArrayList<ArrayList<Integer>> list = new ArrayList<>();

        int b = 0; // buy index
        int s = 0; // sell index

        // check if we can sell at current position
        while(s <= n-2){

            // if next position is higher, move sell to the next position
            if(A[s+1] > A[s]){
                s++;

            // otherwise, we may have a segment if b != s (buy/sell the same stock on same day)
            }else{

                if(b != s){
                    ArrayList<Integer> segment = new ArrayList<>();
                    segment.add(b);
                    segment.add(s);
                    list.add(segment);
                }

                // we can try buying the next stock
                // reset both buy and sell to the next stock position
                b = s+1;
                s = b;

            }

        }

        // handle last segment
        // example: {4,2,2,2,4}
        // we would buy at index 3
        // sell pointer will increase to next index 4
        // but then the loop will end, so this segment will not get added
        if(b != s){
            ArrayList<Integer> segment = new ArrayList<>();
            segment.add(b);
            segment.add(s);
            list.add(segment);
        }

        return list;

    }

}
```

## BETTER WAY TO CODE

- find local minima
- find local maxima after previous local minima
- if such a pair exists, add to answer
- reset to next position

```java
class Solution {

    // Function to find the days  buying and selling stock for max profit.
    ArrayList<ArrayList<Integer>> stockBuySell(int A[], int n) {

        ArrayList<ArrayList<Integer>> list = new ArrayList<>();

        int b = 0; // buy index
        int s = 0; // sell index

        while (b < n - 1) {

            // Move 'b' to the next local minima (if current price is not the local minima).
            while (b < n - 1 && A[b + 1] <= A[b]) {
                b++;
            }

            s = b; // start searching for the next local maxima from 'b'

            // Move 's' to the next local maxima (if current price is not the local maxima).
            while (s < n - 1 && A[s + 1] > A[s]) {
                s++;
            }

            // If 'b' is less than 's', it means we have a valid buy-sell pair.
            if (b < s) {
                list.add(new ArrayList<>(Arrays.asList(b, s)));
            }

            // Move 'b' to 's + 1' to start the search for the next potential buy day.
            b = s + 1;
        }

        return list;
    }
}
```
