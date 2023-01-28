---
layout: post
title: Jump Game II
date: 2023-01-26 23:51 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

```text
    0 <= j <= nums[i] and
    i + j < n
```

Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

[leetcode](https://leetcode.com/problems/jump-game-ii/description/)

### SOLUTION

```java
class Solution {

    public int jump(int[] nums) {

        int n = nums.length;

        //current goal
        int current = n-1;

        //total jumps
        int jumps = 0;

        //while we have not reached the start position which is index 0
        while(current != 0){

            //since we are not at 0, at least one more jump is needed
            jumps++;

            //init for next goal
            int next = current-1;
            
            //get the left most index from which we can reach the current goal
            for(int i=current-1; i>=0; i--){

                //if current position + max jump possible is >= current goal, it's a possible next goal
                //continue to check for other left position
                //finally, we will have the left most index from which we can reach the current goal
                if(i + nums[i] >= current){
                    next = i;
                }
            }

            //update the next goal
            current = next;

        }

        return jumps;

    }

}
```

### SECOND APPROACH (SIMPLIFIED BFS)

[NeetCode YouTube](https://www.youtube.com/watch?v=dJ7sWiOoK7g)

```java
class Solution {

    public int jump(int[] nums) {

        int n = nums.length;
        int jumps = 0;

        //window start to end to store the start and end of a "level/window".
        int l=0, r=0;

        //until we are at end index, keep updating L and R (next level indices)
        while(r<n-1){

            //since we've not reached the destination yet, at least one more jump is needed
            jumps++;    

            //init
            int maxJumpPossible = r+1;

            //check the max jump possible for the current windows
            for(int i=l; i<=r; i++){
                maxJumpPossible = Math.max(maxJumpPossible, i+nums[i]);
            }

            //next window/level will start from (R+1) to (maxJumpPossible from the previous level)
            l=r+1;
            r = maxJumpPossible;

        }

        return jumps;

    }

}
```
