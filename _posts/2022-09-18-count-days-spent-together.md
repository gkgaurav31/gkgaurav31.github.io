---
layout: post
title: Count Days Spent Together
date: 2022-09-18 00:27 +0530
author: "Gaurav Kumar"
tags: "java contest_question"
---

### PROBLEM DESCRIPTION

Alice and Bob are traveling to Rome for separate business meetings.

You are given 4 strings arriveAlice, leaveAlice, arriveBob, and leaveBob. Alice will be in the city from the dates arriveAlice to leaveAlice (inclusive), while Bob will be in the city from the dates arriveBob to leaveBob (inclusive). Each will be a 5-character string in the format "MM-DD", corresponding to the month and day of the date.

Return the total number of days that Alice and Bob are in Rome together.

You can assume that all dates occur in the same calendar year, which is not a leap year. Note that the number of days per month can be represented as: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31].

### SOLUTION

[Leet Code](https://leetcode.com/problems/count-days-spent-together/)

```java
class Solution {
    public int countDaysTogether(String arriveAlice, String leaveAlice, String arriveBob, String leaveBob) {
        
        int[] m = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        
        //Convert each MM-DD to relative day from beginning of the year. 
        int aa = calcDays(arriveAlice, m);
        int la = calcDays(leaveAlice, m);
        int ab = calcDays(arriveBob, m);
        int lb = calcDays(leaveBob, m);
        
        //Common Day = DaysTillTheFirstLeave - DaysTillTheSecondArrival + 1 
        //This is because we need common days, which will start only after 2nd person starts and end when any person leaves
        int commonDays = Math.min(la, lb) - Math.max(aa, ab) + 1;
        
        return Math.max(0, commonDays);
        
    }
    
    public int calcDays(String s, int[] m){
        
        int count=0;
        
        int month = Integer.parseInt(s.substring(0,2));
        int day = Integer.parseInt(s.substring(3));
        
        //If month is May == 5th month, we take all days for the first 4 months
        //This is from index 0 to 3
        for(int i=0; i<month-1; i++){
            count+=m[i];
        }
        
        return count+day;
        
    }
    
}
```
