---
layout: post
title: Alien Dictionary (geeksforgeeks - SDE Sheet)
date: 2024-10-16 13:22 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.
Note: Many orders may be possible for a particular test case, thus you may return any valid order and output will be 1 if the order of string returned by the function is correct else 0 denoting incorrect string returned.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/heap-sort/1?page=3)

## SOLUTION

We can solve this by treating the problem as a graph where each character represents a node. The relationships between characters (determined by their order in the provided words) form directed edges between these nodes.

We start by comparing adjacent words in the list to identify which characters come before others. For example, if the first differing character in "abc" and "acd" is 'b' and 'c', we can say 'b' comes before 'c'. We build an adjacency list to represent these relationships and keep track of how many edges point to each character using an indegree array (for topological sort, which will give us the required result String).

Finally, we perform a topological sort, which allows us to determine a valid order of characters by processing nodes with zero indegree first. If we can process all characters, we return the constructed order; if not, it indicates a cycle in the graph, meaning a valid character order is impossible.

```java
class Solution {

    public String findOrder(String[] dict, int n, int k) {

        // we will try to create a graph for the k characters
        // we will represent first k alphabets using index [0, k-1]

        // adj list for graph:
        List<Set<Integer>> adj = new ArrayList<>();

        for(int i=0; i<k; i++)
            adj.add(new HashSet<>());

        // go through the list of words and use that to create an edge in the graph
        // let's say the words are: baa, abcd
        // we can say for sure that b < a
        // so create an edge from b -> a
        // we will also track indegree for topological sort later which will help in creating the final string

        int[] indegree = new int[k];

        // create the adj list and indegree
        buildGraph(dict, adj, indegree, n);

        return topologicalSort(adj, indegree);

    }

    public String topologicalSort(List<Set<Integer>> adj, int[] indegree){

        StringBuilder sb = new StringBuilder();

        // Queue for topological sort
        Queue<Integer> q = new LinkedList<>();

        // Add all characters with indegree 0 to the queue
        for(int i=0; i<indegree.length; i++){
            if(indegree[i] == 0)
                q.add(i);
        }

        // Count of processed characters
        int count = 0;

        while(!q.isEmpty()){

            // Get the character with indegree 0
            int x = q.poll();

            // Append the character to the result
            sb.append((char)(x + 'a'));

            // Increment the count of processed characters
            count++;

            // Reduce the indegree for each neighboring character
            for(Integer n: adj.get(x)){

                indegree[n]--;

                // If indegree of neighbor becomes 0, add it to the queue
                if(indegree[n] == 0)
                    q.add(n);

            }

        }

        // If not all characters were processed, there was a cycle
        // Return empty string indicating no valid order
        if(count != indegree.length)
            return "";

        return sb.toString();

    }

    public void buildGraph(String[] words, List<Set<Integer>> adj, int[] indegree, int n){

        // Iterate through the list of words to build the graph
        for(int i=0; i<n-1; i++){

                String s1 = words[i];
                String s2 = words[i+1];

                // Determine the minimum length of the two words
                int minLength = Math.min(s1.length(), s2.length());

                // Compare characters of both words to find the first differing character
                for(int k=0; k<minLength; k++){

                    if(s1.charAt(k) != s2.charAt(k)){

                        // Character index of s1
                        int from = s1.charAt(k) - 'a';

                        // Character index of s2
                        int to = s2.charAt(k) - 'a';

                        // same relation can be found using multiple words
                        // indegree will not change if it was already checked before
                        if(!adj.get(from).contains(to)){

                            // Add the directed edge from 'from' to 'to'
                            adj.get(from).add(to);

                            // Increment the indegree of 'to'
                            indegree[to]++;

                        }

                        // Break after finding the first differing character
                        break;

                    }

                }

        }

    }
}
```
