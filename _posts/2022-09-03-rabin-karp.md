---
layout: post
title: Rabin Karp
date: 2022-09-03 02:38 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing string geeksforgeeks"
categories: "string"
---

## Problem Description

Check if pattern p exists in string s.
[Rabin Karp](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/pattern_matching_rabin_karp.jpg)

```java
package com.gauk;

public class RabinKarp {

    private static final int M = 1000000007;
    private static final int prime = 23;

    public static void main(String[] args) {
        String str = "abcbagdda";
        String pattern = "bagd";

        System.out.println(new RabinKarp().matchPattern(str, pattern));
    }

    public boolean matchPattern(String s, String pattern){

        if(s.length() < pattern.length()) return false; //pattern length cannot be more than string length

        int w = pattern.length(); //window size
        long p = prime;

        //Initialization:

        //Convert pattern to number
        long fw = pattern.charAt(0);
        for(int i=1; i<w; i++){
            fw = (fw%M + (pattern.charAt(i)%M*p%M)%M)%M;
            p=(p%M*prime%M)%M;
        }

        //First window
        p = prime;
        long ft = s.charAt(0);
        for(int i=1; i<w; i++){
            ft = (ft%M + (s.charAt(i)%M*p%M)%M)%M;
            p=(p%M*prime%M)%M;
        }

        if(fw == ft) return true; //If the first window matches the pattern

        int start = 1;
        int end = start+w-1;
        long p2 = 1;
        while(end < s.length()){

            String substring = s.substring(start, end+1); //end+1 because substring method excludes end index passed

            //Calculate int value for substring. Remove element before first string and add last element power with p^index
            ft = ((ft - (s.charAt(start-1)%M*p2%M)%M+M)%M + (s.charAt(end)%M*p%M)%M)%M;

            p = (p*prime)%M;
            p2 = (p2*prime)%M;

            //Multiply int value of pattern by p. We will use this to compare with current window's int value.
            fw = (fw * prime)%M;

            if(ft == fw) return true;

            start++;
            end++;

        }

        return false;

    }


}
```
