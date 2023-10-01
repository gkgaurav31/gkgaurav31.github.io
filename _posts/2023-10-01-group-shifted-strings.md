---
layout: post
title: Group Shifted Strings
date: 2023-10-01 14:57 +0530
author: "Gaurav Kumar"
tags: "java hashing leetcode leetcodealgo100"
categories: "hashing"
---

## PROBLEM DESCRIPTION

Given a string s and an integer k, return the number of substrings in s of length k with no repeated characters.

[leetcode](https://leetcode.com/problems/find-k-length-substrings-with-no-repeated-characters/)

## SOLUTION

- We find how many shifts it would take if we had to change the first character to 'a'
- We shift all the other character with the same amount
- We use this new string as a key in HashMap. This will help in grouping string which form the same shifted string

```java
class Solution {

    public List<List<String>> groupStrings(String[] strings) {

        int n = strings.length;

        // For all strings in the same kind, if we shift is in a way that the first character is a, that can be used as a unique key
        // The corresponding value will be the actual string to be used in the answer
        // For example:
        // [abc, bcd, xyz] -> all the string belong to the same group
        // abc -> can be changed to abc with 0 shift
        // bcd -> can be changed to abc with 1 shift
        // xyz -> can be changed to abc with (int) x-'a' shifts
        // so, we can use 'abc' as the key to group them together
        Map<String, List<String>> map = new HashMap<>();

        // all each string
        for(int i=0; i<n; i++){

            // get the current string
            String current = strings[i];

            // calculate the string which is formed after shifting them such that the first character should become 0
            String hashKey = calculateHash(current);

            // if that key is not present, new a new list for this unique key
            if(!map.containsKey(hashKey)){
                map.put(hashKey, new ArrayList<String>());
            }

            // append the current string to the key
            map.get(hashKey).add(current);

        }

        // go through all the values in the hashmap and create a List<List<String>> for final result to be returned
        List<List<String>> ans = new ArrayList<>();
        for(List<String> list: map.values()){
            ans.add(list);
        }

        return ans;

    }

    public String calculateHash(String s){

        // how many shifts are needed to change the first character to 'a'?
        int shift = s.charAt(0) - 'a';

        // the string which will be formed if we shift all characters by the number of times calculated above
        StringBuffer sb = new StringBuffer();

        // find the next character after shifting
        for(int i=0; i<s.length(); i++){

            // get the current character
            char ch = s.charAt(i);

            // get the character it will become after shifting
            // tricky test case: ["az","ba"] -> both belong to the same group
            char updatedCh = (char)((ch - shift + 26)%26);

            // append it to the updated string
            sb.append(updatedCh);

        }

        return sb.toString();

    }


}
```
