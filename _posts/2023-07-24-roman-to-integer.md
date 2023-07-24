---
layout: post
title: Roman to Integer
date: 2023-07-24 12:14 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given a roman numeral, convert it to an integer.

[leetcode](https://leetcode.com/problems/roman-to-integer/)

## SOLUTION

### APPROACH 1

If there are two characters left in the string s AND the integer value of first roman character is lesser than the second character:  
    - Add (value of char 2 - value of char 1) to the total  
Else:  
    - Add (value of current character) to the total

```java
class Solution {
    
    public int romanToInt(String s) {

        int n = s.length();

        Map<String, Integer> map = new HashMap<>();
        map.put("M",1000);
        map.put("D",500);
        map.put("C",100);
        map.put("L",50);
        map.put("X",10);
        map.put("V",5);
        map.put("I",1);
        
        int total = 0;

        int idx = 0;

        while(idx < n){

            if(idx <= n-2 && ( map.get(s.substring(idx, idx+1)) < map.get(s.substring(idx+1, idx+2))   ) ){
                total +=  map.get(s.substring(idx+1, idx+2)) - map.get(s.substring(idx, idx+1)) ;
                idx = idx + 2;
            }else{
                total += map.get(s.substring(idx, idx+1));
                idx++;
            }

        }

        return total;

    }

}
```

### OPTIMIZATION 1

There are certain specific representations possible for two roman literals are combines. We can include then in the hash map to make the calculations simpler.

```java
class Solution {
    
    public int romanToInt(String s) {

        int n = s.length();

        Map<String, Integer> map = new HashMap<>();
        map.put("M",1000);
        map.put("D",500);
        map.put("C",100);
        map.put("L",50);
        map.put("X",10);
        map.put("V",5);
        map.put("I",1);

        map.put("CM", 900);
        map.put("CD", 400);
        map.put("XC", 90);
        map.put("XL", 40);
        map.put("IX", 9);
        map.put("IV", 4);
        
        int total = 0;

        int idx = 0;

        while(idx < n){

            if(idx <= n-2 && map.containsKey(s.substring(idx, idx+2)) ){
                total +=  map.get(s.substring(idx, idx+2));
                idx = idx + 2;
            }else{
                total += map.get(s.substring(idx, idx+1));
                idx++;
            }

        }

        return total;

    }

}
```

### OPTIMIZATION 2

In approach 1, we were traversing from left to right and we had to lookup two characters ahead to check if the current number should be added or subtracted.  

If we instead traverse from right to left, the current number will be subtracted only if it's lesser than it's right (previousy visited) number. Otherwise, it must be added.  

This way, we can use a simple loop to calculate the total.

```java
class Solution {
    
    public int romanToInt(String s) {

        int n = s.length();

        Map<String, Integer> map = new HashMap<>();
        
        map.put("M",1000);
        map.put("D",500);
        map.put("C",100);
        map.put("L",50);
        map.put("X",10);
        map.put("V",5);
        map.put("I",1);

        int total = map.get(s.substring(n-1));

        for(int i=n-2; i>=0; i--){

            int current = map.get(String.valueOf(s.charAt(i)));
            int right = map.get(String.valueOf(s.charAt(i+1)));

            if(current < right){
                total -= current;
            }else{
                total += current;
            }

        }

        return total;

    }

}
```
