---
layout: post
title: Stock span problem (geeksforgeeks - SDE Sheet)
date: 2024-08-25 14:31 +0530
author: "Gaurav Kumar"
tags: "java stack arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The stock span problem is a financial problem where we have a series of n daily price quotes for a stock and we need to calculate the span of stocks price for all n days.
The span Si of the stocks price on a given day i is defined as the maximum number of consecutive days just before the given day, for which the price of the stock on the given day is less than or equal to its price on the current day.
For example, if an array of 7 days prices is given as {100, 80, 60, 70, 60, 75, 85}, then the span values for corresponding 7 days are {1, 1, 1, 2, 1, 4, 6}.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/stock-span-problem-1587115621/1?page=2)

## SOLUTION

If we know the spans of all the consecutive previous stock from the current stocks which have <= price, then it can add all of them to get the current span.

- Start with the first stock, which has a span of 1 because there are no previous stocks to compare it with.

For each subsequent stock:

- Set the initial span to 1 (since each stock always has a minimum span of 1, which is the stock itself).

- While the stack is not empty and the price of the stock at the top of the stack is less than or equal to the current stock’s price, it means that the current stock price is greater than or equal to those on the stack. Therefore, the span of the current stock should include the span of those stocks.
- Add the span of the stock that is being popped from the stack to the current stock’s span. This is because all the stocks that contributed to the popped stock’s span are also less than or equal to the current stock price.
- After including the span of the popped stock, pop it from the stack since it’s no longer needed for future comparisons.

- Once the stack is empty or the top stock on the stack has a price greater than the current stock, push the current stock price and its span onto the stack.

```java
class Solution {

    public static int[] calculateSpan(int price[], int n) {

        // Create a stack to keep track of the prices and their spans.
        Stack<int[]> stack = new Stack<>();

        int[] res = new int[n];

        // Initialize the stack with the first day's price and its span (1).
        stack.push(new int[]{price[0], 1});

        // The span for the first day is always 1.
        res[0] = 1;

        // Iterate through the prices starting from the second day.
        for(int i = 1; i < n; i++){

            int span = 1;  // Start with a span of 1 for the current day.

            // While there are elements in the stack and the current day's price is
            // greater than or equal to the price at the top of the stack,
            // pop the stack and add the span of the popped element to the current span.
            while(stack.size() > 0 && stack.peek()[0] <= price[i]){

                // Add the span of the element being popped.
                span += stack.peek()[1];

                // Remove the element from the stack.
                stack.pop();
            }

            // Push the current day's price and its calculated span onto the stack.
            stack.push(new int[]{price[i], span});

            res[i] = span;

        }

        return res;

    }

}
```
