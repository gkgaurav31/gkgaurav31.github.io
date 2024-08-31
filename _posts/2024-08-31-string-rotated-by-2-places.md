---
layout: post
title: String Rotated by 2 Places (geeksforgeeks - SDE Sheet)
date: 2024-08-31 13:02 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings a and b. The task is to find if the string 'b' can be obtained by rotating (in any direction) string 'a' by exactly 2 places.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/check-if-string-is-rotated-by-two-places-1587115620/1?page=4)

## SOLUTION

```java
class Solution
{
    //Function to check if a string can be obtained by rotating
    //another string by exactly 2 places.
    public static boolean isRotated(String str1, String str2)
    {

        int n = str1.length();

        // not possible to rotate by two steps
        if(str2.length() != n || n < 3)
            return false;

        // to check chars for str1
        int i = 0;

        // if we rotate clockwise by two step, the characters will start from index 2 in str2
        int j = 2;

        // init
        boolean possible = true;

        // check if char at i in str1 is matching char at j in str2
        // it's possible that j will go out of range
        // that time we want to take %n to find the corresponding index
        while(i < n){
            if(str1.charAt(i) != str2.charAt(j%n)){
                possible = false;
                break;
            }
            i++; j++;
        }

        // if clockwise is still true, then we found that clockwise 2 step is possible
        if(possible)
            return true;

        // otherwise, try anti-clockwise in the same way
        // the first char will at n-2 index in str2
        i = 0;
        j = n-2;
        while(i < n){

            if(str1.charAt(i) != str2.charAt(j%n))
                return false;

            i++; j++;
        }

        return true;

    }

}
```
