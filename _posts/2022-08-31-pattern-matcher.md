---
layout: post
title: Pattern Matcher
date: 2022-08-31 10:01 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing strings geeksforgeeks"
categories: "strings"
---

## Problem Description

Check if pattern p exists in string s.

### Solution

```java
public boolean checkForPattern(String s, String p){

    String str = p + "$" + s;

    int[] lps = LPS.getLPS(str);

    for(int i=0; i<lps.length; i++){
        if(lps[i] == p.length()){
            return true;
        }
    }

    return false;

}
```
