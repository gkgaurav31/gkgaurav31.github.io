---
layout: post
title: Coin Change
date: 2022-11-22 23:37 +0530
---

### BRUTE FORCE

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        return coinHelper(coins, amount);   
    }

    public int min = Integer.MAX_VALUE;

    public int coinHelper(int[] coins, int amount){

        //0 coins needed for amount 0
        if(amount == 0) return 0;

        //if amount is -ve, it's not possible
        if(amount < 0) return -1;

        int c = Integer.MAX_VALUE;

        //loop through each coin
        for(int i=0; i<coins.length; i++){

            //get count of coins for amount-coins[i]
            int count = coinHelper(coins, amount - coins[i]);

            //if amount-coins[i] is not possible, skip
            if(count == -1) continue;

            //update min value
            c = Math.min(c, count + 1); //added 1 because we used one coin by subtracting from amount
        }

        //if c is still Integer.MAX_VALUE, we did not find any possible combination of coins
        return (c == Integer.MAX_VALUE?-1:c);

    }
}
```
