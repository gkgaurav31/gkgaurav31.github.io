---
layout: post
title: Minimum Window Substring
date: 2023-03-08 23:55 +0530
author: "Gaurav Kumar"
tags: "java leetcode sliding_window neetcode150"
categories: "neetcode150"
---

### Problem Description

Given two strings s and t of lengths m and n respectively, return the minimum window
substring
of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

[leetcode](https://leetcode.com/problems/minimum-window-substring/description/)

### Solution

[NeetCode YouTube](https://www.youtube.com/watch?v=jSto0O4AJbM)

```java
class Solution {

    public String minWindow(String s, String t) {

        //get frequency of each character in the string t
        Map<Character, Integer> countT = getFrequency(t);

        //we will use this hashmap window to maintain the frequency of characters in our window
        Map<Character, Integer> window = new HashMap<>();

        //have -> how many characters (along with their frequencies) have been found
        //need -> how many matches for unique characters do we need?
        int have = 0; int need = countT.size();

        //track left and right index for the window for possible answer
        //we can also keep a substring but this is more optimal
        int ansL=-1;
        int ansR=-1;
        int ansLength=Integer.MAX_VALUE;
        
        //left of the window
        int l = 0;

        //r -> right of the window
        //keep increasing the size of the windows towards right by including characters from string s
        for(int r=0; r<s.length(); r++){
            
            //get current character
            Character current = s.charAt(r);

            //increase its frequency in the window
            window.put(current, window.getOrDefault(current,0)+1);

            //if the current character belongs to target string
            //AND
            //we have the required number of current characters (since duplicates also need to be considered)
            if(countT.containsKey(current) && countT.get(current).intValue() == window.get(current).intValue()){

                //since we have matched the requirement of this character, increase "have" count
                have++;
            }

            //IMPORTANT:
            //now, we already have a possible answer, but we will try to find if there is a better answer by reducing the window size
            //At this point, we will try to reduce the window size from the left
            //We will keep doing this, until "have" and "need" match (which means, the requirements of all the characters and their frequencies meet)
            while(have == need){
                
                //current window size
                int currentWindowLength = r - l + 1;

                //if it is lesser than what we have got earlier, update the answer
                if(currentWindowLength < ansLength){
                    ansL = l;
                    ansR = r;
                    ansLength = r - l + 1;
                }

                //get the character on the left most size of the window
                Character leftChar = s.charAt(l);

                //reduce its frequency in our window by 1 (we are trying to see if we still have the required number of characters matching the requirement even after reducing the frequency of the left most character)
                window.put(leftChar, window.get(leftChar) - 1);

                //if the left most character is present in target string
                //AND
                //frequency of that character becomes lesser than how much is needed
                if(countT.containsKey(leftChar) && window.get(leftChar).intValue() < countT.get(leftChar).intValue()){

                    //we reduce our "have" count because now we have one unsatisfied requirement
                    have--;
                }

                //if the above condition does not match, meaning that the requirements are still as needed even after removing the left most character in the window, we will reduce the windows size again from the left
                l++;

            }

        }

        //no substring found with all needed characters with their corresponding frequencies
        if(ansLength == Integer.MAX_VALUE) return "";

        return s.substring(ansL, ansR+1);
        

    }

    public Map<Character, Integer> getFrequency(String s){

        Map<Character, Integer> map = new HashMap<>();

        for(int i=0; i<s.length(); i++){
            map.put(s.charAt(i), map.getOrDefault( s.charAt(i) , 0) + 1);
        }

        return map;

    }

}
```
