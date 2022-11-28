---
layout: post
title: Find Players With Zero or One Losses
date: 2022-11-28 23:04 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode"
categories: "arrays"
---

## Problem Description

You are given an integer array matches where matches[i] = [winneri, loseri] indicates that the player winneri defeated player loseri in a match.  

Return a list answer of size 2 where:  

- answer[0] is a list of all players that have not lost any matches.
- answer[1] is a list of all players that have lost exactly one match.

The values in the two lists should be returned in increasing order.  

Note:  

- You should only consider the players that have played at least one match.
- The testcases will be generated such that no two matches will have the same outcome.

[leetcode](https://leetcode.com/problems/find-players-with-zero-or-one-losses/description/)

### Solution

```java
class Solution {
    public List<List<Integer>> findWinners(int[][] matches) {

        Map<Integer, Integer> winners = new HashMap<>();
        Map<Integer, Integer> losers = new HashMap<>();

        //track the number of times a player wins or loses
        for(int i=0; i<matches.length; i++){
            
            int p1 = matches[i][0];
            int p2 = matches[i][1];
            
            if(!winners.containsKey(p1)){
                winners.put(p1, 1);
            }else{
                winners.put(p1, winners.get(p1)+1);
            }

            if(!losers.containsKey(p2)){
                losers.put(p2, 1);
            }else{
                losers.put(p2, losers.get(p2)+1);
            }

        }

        //not lost any match
        ArrayList<Integer> noLoss = new ArrayList<>();
        for(java.util.Map.Entry<Integer,Integer> w: winners.entrySet()){
            if(!losers.containsKey(w.getKey())){
                noLoss.add(w.getKey());
            }
        }

        //single loss
        ArrayList<Integer> singleLoss = new ArrayList<>();
        for(java.util.Map.Entry<Integer,Integer> l: losers.entrySet()){
            if(l.getValue() == 1) singleLoss.add(l.getKey());
        }

        Collections.sort(noLoss);
        Collections.sort(singleLoss);
        
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        ans.add(noLoss);
        ans.add(singleLoss);
        
        return ans;
        
    }
}
```
