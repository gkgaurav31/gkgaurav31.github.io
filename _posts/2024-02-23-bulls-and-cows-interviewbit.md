---
layout: post
title: Bulls and Cows (InterviewBit)
date: 2024-02-23 23:31 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are playing the Bulls and Cows game with your friend.

You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:

The number of "bulls", which are digits in the guess that are in the correct position.
The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position.
Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.

Given the secret number secret and your friend's guess guess, return the hint for your friend's guess.

The hint should be formatted as "xAyB", where x is the number of bulls and y is the number of cows. Note that both secret and guess may contain duplicate digits.

## SOLUTION

```java
public class Solution {

    public String solve(String A, String B) {

        int n = A.length(); // secret and guess length are same

        // freq map of secret string
        Map<Character, Integer> map = new HashMap<>();
        for(int i=0; i<n; i++){
            map.put(A.charAt(i), map.getOrDefault(A.charAt(i), 0) + 1);
        }

        int bull = 0;
        int cow = 0;

        // update the bulls if next char of A and B are same
        // reduce its frequency in map
        // if it becomes 0, remove it
        for(int  i=0; i<n; i++){

            char ch1 = A.charAt(i);
            char ch2 = B.charAt(i);

            if(ch1 == ch2){
                bull++;
                map.put(ch1, map.get(ch1)-1);

                if(map.get(ch1) == 0)
                    map.remove(ch1);
            }

        }


        // track cows now
        for(int i=0; i<n; i++){

            char ch1 = A.charAt(i);
            char ch2 = B.charAt(i);

            // if both are same, it would have been included in bulls already
            if(ch1 == ch2) continue;

            // otherwise check if it is present anywhere else
            // for that we can just check if it still exists in the map (which means it must have > 0 frequency)
            if(map.containsKey(ch2)){

                // increase cow count
                cow++;

                // reduce the freq of current char
                map.put(ch2, map.get(ch2)-1);

                // if it becomes 0, remove it
                if(map.get(ch2) == 0)
                    map.remove(ch2);

            }

        }


        return bull + "A" + cow + "B";

    }

}
```
