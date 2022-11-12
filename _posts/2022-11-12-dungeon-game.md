---
layout: post
title: Dungeon Game
date: 2022-11-12 20:54 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming important"
categories: "dynamic_programming"
---

## Problem Description

The demons had captured the princess and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of m x n rooms laid out in a 2D grid. Our valiant knight was initially positioned in the top-left room and must fight his way through dungeon to rescue the princess.  

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.  

Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as 0) or contain magic orbs that increase the knight's health (represented by positive integers).  

To reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.  

Return the knight's minimum initial health so that he can rescue the princess.  

Note that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.  
[leetcode](https://leetcode.com/problems/dungeon-game/description/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/dungeon_game.png)

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        
        int n = dungeon.length;
        int m = dungeon[0].length;

        int[][] dp = new int[n][m];

        //initilize all by -1
        for(int i=0; i<n; i++){
            Arrays.fill(dp[i], -1);
        }

        //min health needed at last cell
        dp[n-1][m-1] = Math.max(1, 1-dungeon[n-1][m-1]);

        minHealthHelper(dungeon, n, m, 0, 0, dp);

        //health needed at the beginning
        return dp[0][0];
    }

    public int minHealthHelper(int[][] dungeon, int n, int m, int i, int j, int[][] dp){
        
        //base case
        if(i == dungeon.length || j == dungeon[0].length){
            //either i or j will be out of bound at any time. So, we can set "a" or "b" to Integer.MAX_VALUE such that only the min value is consider. 
            return Integer.MAX_VALUE;
        }

        //if min health has not been calculated
        if(dp[i][j] == -1){
            
            //min health downwards
            int a = minHealthHelper(dungeon, n, m, i+1, j, dp);

            //min health on the right
            int b = minHealthHelper(dungeon, n, m, i, j+1, dp);

            dp[i][j] = Math.max(1, Math.min(a,b)-dungeon[i][j]);
        }

        return dp[i][j];

    }
}
```
