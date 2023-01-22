---
layout: post
title: Edit Distance
date: 2023-01-22 17:31 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

[leetcode](https://leetcode.com/problems/edit-distance/description/)

### SOLUTION

```java
class Solution {

    public int minDistance(String word1, String word2) {

        //edge case
        if(word1.length() == 0 || word2.length() == 0){
            return Math.max(word1.length(), word2.length());
        }

        return ED(word1, word2, word1.length()-1, word2.length()-1, new HashMap<>());
    }

    public int ED(String s1, String s2, int i, int j, Map<String, Integer> map){
        
        //all characters if s2 and s2 are covered, so no operations needed for empty strings
        if(i == -1 && j == -1) return 0;

        //no characters in s1, so number of operations will be equal to length of second string left
        if(i == -1) return j+1;

        //no characters in s2, so number of operations will be equal to length of first string left
        if(j == -1) return i+1;

        if(!map.containsKey(i+":"+j)){
            
            //characters match
            if(s1.charAt(i) == s2.charAt(j)){
                
                //move both i and j
                map.put(i+":"+j, ED(s1, s2, i-1, j-1, map));

            //else we can try either of the 3 options: insert, delete, replace
            }else{

                //delete
                int x = 1 + ED(s1, s2, i-1, j, map);

                //insert
                int y = 1 + ED(s1, s2, i, j-1, map);

                //replace
                int z = 1 + ED(s1, s2, i-1, j-1, map);

                //take min out of the above 3 options
                map.put(i+":"+j, Math.min(x, Math.min(y, z)) );

            }

        }

        return map.get(i+":"+j);

    }

}
```
