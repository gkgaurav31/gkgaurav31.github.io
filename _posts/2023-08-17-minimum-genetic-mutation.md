---
layout: post
title: Minimum Genetic Mutation
date: 2023-08-17 21:53 +0530
author: "Gaurav Kumar"
tags: "java graphs leetcode leetcode150 important"
categories: "graphs"
---
## PROBLEM DESCRIPTION

A gene string can be represented by an 8-character long string, with choices from 'A', 'C', 'G', and 'T'.
Suppose we need to investigate a mutation from a gene string startGene to a gene string endGene where one mutation is defined as one single character changed in the gene string.

> For example, "AACCGGTT" --> "AACCGGTA" is one mutation.

There is also a gene bank bank that records all the valid gene mutations. A gene must be in bank to make it a valid gene string.

Given the two gene strings startGene and endGene and the gene bank bank, return the minimum number of mutations needed to mutate from startGene to endGene. If there is no such a mutation, return -1.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

[leetcode](https://leetcode.com/problems/minimum-genetic-mutation/)

## SOLUTION

Starting from startGene, we will try to find other "reachable" genes. We can make them as visited and remove them from the list/set so that we do not visit them again. Keep doing this, until we have reached endGene out exhausted all the genes which are reachable. This is basically BFS. The answer is the shorted path to the destination, number of edges. So, we can keep a count of the "levels" and return it as our answer.

```java
class Solution {

    //method to check if two string a and b representing two genes are valid mutations
    //they must have 1 character different at least and at most.
    public boolean valid(String a, String b){

        if(a.equals(b)) return false;

        int diff = 0;

        for(int i=0; i<a.length(); i++){

            if(a.charAt(i) != b.charAt(i)) diff++;

            if(diff >= 2) return false;

        }

        return true;

    }
    
    public int minMutation(String startGene, String endGene, String[] bank) {

        //add all possible genes to a hashset 
        Set<String> possibleMutations = new HashSet<>();
        for(String s: bank) 
            possibleMutations.add(s);
        
        //startGene is already visited, so we can remove it from the next possible set of mutations possible
        possibleMutations.remove(startGene);

        //queue for BFS
        //add the start gene
        Queue<String> q = new LinkedList<>();
        q.offer(startGene);

        //number of steps to reach endGene
        int mutations = 0;
        
        //while there are no more genes left to check
        while(!q.isEmpty()){
            
            //increase number of steps
            mutations++;

            //get the number of elements/genes in the current "level"
            int elements = q.size();

            //get the genes in the current level and find their next possible mutations
            for(int i=0; i<elements; i++){
                
                //get current gene from the queue
                String currentGene = q.poll();

                //create a set of visited genes
                //we will use this to remove it from the previous hashset to avoid going back to the same string gene again
                Set<String> visited = new HashSet<>();

                //go through the list of genes and look for valid mutations
                for(String s: possibleMutations){
                    
                    //if the mutation is valid, add it to the queue
                    //mark it as visited
                    //if the next gene is found to be the endGene, we have can return the number of mutations here
                    if(valid(currentGene, s)){
                        
                        q.offer(s);
                        visited.add(s);

                        if(s.equals(endGene)) return mutations;

                    }
                }

                //remove the visited genes from the list of genes
                possibleMutations.removeAll(visited);

            }
            
        }

        return -1;

    }

}
```
