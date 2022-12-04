---
layout: post
title: Time Based Key-Value Store
date: 2022-12-04 15:01 +0530
author: "Gaurav Kumar"
tags: "java binary_search neetcode150"
categories: "neetcode150"
---

## Problem Description

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the TimeMap class:

- TimeMap() Initializes the object of the data structure.
- void set(String key, String value, int timestamp) Stores the key key with the value value at the given time timestamp.
- String get(String key, int timestamp) Returns a value such that set was called previously, with timestamp_prev <= timestamp. If there are multiple such values, it returns the value associated with the largest timestamp_prev. If there are no values, it returns "".

[leetcode](https://leetcode.com/problems/time-based-key-value-store/description/)

### Solution

A single way to implement this is by using: ```Map<Key, Map<Timestamp, Value>> map;```. While searching for the value at a specific timestamp for a key, we would need to iterate through the timestamp till 1 to see which is nearest timestamp (which has a value set before) and return that. This will lead to TLE. To optimize it, we can use Sorted HashMap for storing timestamp and its value: ```Map<String, TreeMap<Integer, String>> map;```. Then, all we need to find is the nearest timestamp for the input timestamp which is set. This can be done using an in-built method in Java - Integer floorKey = map.get(key).floorKey(timestamp);.

```java
class TimeMap {

    Map<String, TreeMap<Integer, String>> map;

    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        if(!map.containsKey(key)){
            
            TreeMap<Integer, String> t = new TreeMap<>();
            t.put(timestamp, value);

            map.put(key, t);
        }else{
            Map<Integer, String> t = map.get(key);
            t.put(timestamp, value);
        }
    }
    
    public String get(String key, int timestamp) {

        if(!map.containsKey(key)) return "";
        
        //returns a key equal to or less than searched key or null if no such key exists that satisfies the above condition.
        Integer floorKey = map.get(key).floorKey(timestamp);

        // Return searched time's value, if exists.
        if (floorKey != null) {
            return map.get(key).get(floorKey);
        }
        
        return "";

    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```
