---
layout: post
title: Alien Dictionary
date: 2023-03-05 21:30 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150 important"
categories: "neetcode150"
---

### Problem Description

There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings words from the alien language's dictionary, where the strings in words are
sorted lexicographically
by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

[leetcode](https://leetcode.com/problems/alien-dictionary/description/)

### Solution

The following code uses indegree to solve this problem. The below code is not so optimized. The problem involves a lot of edge cases which we need to handle but the basic idea is simple to understand.

First we can compare each word with other words, and try to get some relation between two characters which are different. Since the words are sorted lexicographically, we can make this judgement based on the first character which is different in the two words. Example: apple and apps. Here, "app" is same, but "l" and "s" don't match. So, we can say that "l" must come before "s" in the alien languge. Using this idea, we can form the adjacency list.

While checking this, we also calculate the indegee of all the characters which are present in the list of words. Then, pick up all the characters which have 0 indegree. Having 0 indegree means that they don't depend on any other character and they can be included in our string. After including that character, consider that it can be deleted from the graph (we need unique characters in our answer string). That means, all the nodes which had an incoming edge from this particular node will have a reduced indegree. So, reduce the indegree of all its neighbours by 1. If this was the last neighbour, the indegree would become 0, in which case, we will include the current node as the next character.

After the iterations, if the indegree of any node is more than 0, that means that there is a cycle. (we can return "")

```java
class Solution {


    public String alienOrder(String[] words) {

        int n = words.length;

        //adjacency list
        Map<String, Set<String>> map = new HashMap<>();

        //indegree for all characters
        Map<String, Integer> indegree = new HashMap<>();

        //init: add all character and set indegree as 0 for now
        for(int i=0; i<words.length; i++){
            String c = words[i];
            for(int j=0; j<c.length(); j++){
                indegree.put(c.charAt(j)+"", 0);
            }
        }

        //to check if an edge has been already included
        Set<String> set = new HashSet<>();

        //for each word, compare it with all other words
        for(int i=0; i<n; i++){

            for(int j=i+1; j<n; j++){
                
                //if both words are same, continue
                if(words[i].equals(words[j])) continue;

                //if 1st word is a prefix of the 2nd word and they are not exactly equal, continue
                if(words[j].startsWith(words[i]) && !words[i].equals(words[j])) continue;

                //if the 2nd word is a present of 1st word and they are not exactly equal, return "" since it's invalid
                //example: abcde abcd
                if(words[i].startsWith(words[j]) && !words[i].equals(words[j])) return ""; //edge case

                //get the first characters which are not matching
                String[] edge = check(words[i], words[j]);

                //edge: u -> v
                String u = edge[0];
                String v = edge[1];

                //if it has not been visited already
                if(!set.contains(u+":"+v)){
                    
                    //increase the indegree of v by 1
                    indegree.put(v, indegree.getOrDefault(v, 0)+1);

                    //mark visited
                    set.add(u+":"+v);
                }

                //include this in our graph
                if(map.get(u) == null){
                    Set<String> t = new HashSet<>();
                    t.add(v);
                    map.put(u, t);
                }else{
                    map.get(u).add(v);
                }


            }
        }

        //answer string
        StringBuffer sb = new StringBuffer();

        //queue to visit all possible nodes 
        PriorityQueue<String> pq = new PriorityQueue<>();

        //init: add all nodes with indegree == 0 (since they don't have any dependencies)
        for(java.util.Map.Entry e: indegree.entrySet()){
            if((int) e.getValue() == 0){
                pq.add((String)e.getKey());
            }
        }

        //while queue is not empty, check other nodes
        while(!pq.isEmpty()){
            
            //get current node/character
            String current = pq.poll();

            //append to our answer string
            sb.append(current);

            //get its neighbours
            Set<String> neighbours = map.get(current);

            //if there are no neighbours, continue checking other nodes/characters
            if(neighbours == null || neighbours.size() == 0) continue;

            //for each neighbouring character
            for(String s: neighbours){
                
                //reduce the indegree by 1
                indegree.put(s, indegree.get(s)-1);

                //if it becomes 0, this character can be included in our answer string
                //add this to our queue
                if(indegree.get(s) == 0){
                    pq.add(s);
                }

            }

        }

        //cycle check
        //if there is any node with > 0 indegree, there must be a cycle
        for(java.util.Map.Entry e: indegree.entrySet()){
            if((int) e.getValue() > 0){
                return "";
            }
        }

        return sb.toString();

    }

    public String[] check(String w1, String w2){

        int idx=0;

        while(w1.charAt(idx) == w2.charAt(idx)){
            idx++;
        }

        return new String[]{w1.charAt(idx)+"", w2.charAt(idx)+""};

    }

}
```
