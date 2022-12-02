---
layout: post
title: Determine if Two Strings Are Close
date: 2022-12-02 21:31 +0530
author: "Gaurav Kumar"
tags: "java leetcode hashmap"
categories: "hashmap"
---

### Problem Description

Two strings are considered close if you can attain one from the other using the following operations:

- Operation 1: Swap any two existing characters.
        For example, abcde -> aecdb
- Operation 2: Transform every occurrence of one existing character into another existing character, and do the same with the other character.
        For example, aacabb -> bbcbaa (all a's turn into b's, and all b's turn into a's)

You can use the operations on either string as many times as necessary.

Given two strings, word1 and word2, return true if word1 and word2 are close, and false otherwise.

[leetcode](https://leetcode.com/problems/determine-if-two-strings-are-close/description/)

### Solution

```java
class Solution {

    public boolean closeStrings(String word1, String word2) {
        
        //If length of words are different, then they are not "close"
        if(word1.length() != word2.length()) return false;

        //If the character set in the two words are not same, no amount of swapping the position or characters with other characters would help. So return false.
        if(!characterSetMatching(word1, word2)) return false;

        //If there are X characters with frequency Y in word2, we should have same number of characters with that frequency in word2.
        //We don't care if the characters themselves are a mismatch because we can always swap it with other character with the required frequency
        Map<Character, Integer> f1 = new HashMap<>();
        Map<Character, Integer> f2 = new HashMap<>();

        //Track frequency of each chracter in word 1
        for(int i=0; i<word1.length(); i++){
            Character c = word1.charAt(i);
            if(f1.containsKey(c)){
                f1.put(c, f1.get(c)+1);
            }else{
                f1.put(c, 1);
            }
        }

        //Track frequency of each chracter in word 2
        for(int i=0; i<word2.length(); i++){
            Character c = word2.charAt(i);
            if(f2.containsKey(c)){
                f2.put(c, f2.get(c)+1);
            }else{
                f2.put(c, 1);
            }
        }

        //Number of characters with frequency X in f1
        Map<Integer, Integer> f1count = new HashMap<>();

        //Number of characters with frequency X in f2
        Map<Integer, Integer> f2count = new HashMap<>();
        
        for(java.util.Map.Entry e: f1.entrySet()){
            if(f1count.containsKey(e.getValue())){
                f1count.put((Integer)e.getValue(), f1count.get(e.getValue())+1);
            }else{
                f1count.put((Integer)e.getValue(), 1);
            }
        }

        for(java.util.Map.Entry e: f2.entrySet()){
            if(f2count.containsKey(e.getValue())){
                f2count.put((Integer)e.getValue(), f2count.get(e.getValue())+1);
            }else{
                f2count.put((Integer)e.getValue(), 1);
            }
        }

        //If number of characters with frequency X in map1 != number of characters with frequency X in map2, return false
        for(java.util.Map.Entry e: f1count.entrySet()){

            //Count of current frequency
            Integer c1 = (Integer)e.getValue();
            
            //If that frequency is not present in word2, return false
            if(!f2count.containsKey(e.getKey())) return false;

            Integer c2 = f2count.get(e.getKey());

            if(c1 != c2) return false;
        }

        return true;
        
    }

    public boolean characterSetMatching(String s1, String s2){

        Set<Character> set = new HashSet<>();
        for(int i=0; i<s1.length(); i++){
            set.add(s1.charAt(i));
        }

        for(int i=0; i<s2.length(); i++){
            if(!set.contains(s2.charAt(i))) return false;
        }

        return true;

    }

}
```
