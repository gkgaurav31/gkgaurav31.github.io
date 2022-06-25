---
layout: post
title: Group Anagrams
date: 2022-06-25 11:17 +0530
author: "Gaurav Kumar"
tags: "java strings"
categories: "algoexpert"

---

## Problem Description

You have given a list of words. You need to group them together on the basis of whether they are anagrams or not. Two words are anagrams if they have the same alphabets (same frequency and set of alphabets). We are not allowed to use split() or reverse() functions.

## Solution

One way to solve this problem is by using a HashMap - the key will be the sorted value of the strings, because two anagrams will have the same sorted value. We keep checking each word to see if they have the sorted value present as the key. If not, we add a new one. The value will be the actual string (unsorted) since we need to return a list of the actual strings as the result.

```java
import java.util.*;

class Program {

public static String sortString(String s) {
    List<String> list = Arrays.asList(s.split(""));
    Collections.sort(list);
    StringBuilder sb = new StringBuilder("");
    for(String x: list) {
        sb.append(x);
    }
    return sb.toString();
    }
  
  public static List<List<String>> groupAnagrams(List<String> words) {

    Map<String, List<String>> map = new HashMap<>();

    for(int i=0; i<words.size(); i++){
      String c = words.get(i);
      String c_sorted = sortString(c);
      if(map.containsKey(c_sorted)){
        map.get(c_sorted).add(c);
      }else{
        List<String> t = new ArrayList<>();
        t.add(c);
        map.put(c_sorted, t);
      }
    }
    
    List<List<String>> output = new ArrayList<>();
    for(Map.Entry<String, List<String>> entry: map.entrySet()){
      output.add(entry.getValue());
    }
    return output;
  }
}
```