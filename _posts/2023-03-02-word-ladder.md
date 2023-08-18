---
layout: post
title: Word Ladder
date: 2023-03-02 21:57 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

## Problem Description

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

- Every adjacent pair of words differs by a single letter.
- Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
- sk == endWord

Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

[leetcode](https://leetcode.com/problems/word-ladder/description/)

## Solution

### APPROACH 1

[TechDose YouTube](https://www.youtube.com/watch?v=ZVJ3asMoZ18)

```java
class Solution {
    
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        
        //HashSet of the words which are valid
        Set<String> set = new HashSet<>(wordList);

        //If the endWord is not in the list of valid words, we can simply return 0
        if(!set.contains(endWord)) return 0;

        //Queue (for BFS)
        Queue<String> q = new LinkedList<>();

        //init: add the starting word
        q.add(beginWord);

        //init: count number of words transformed
        int count=1;

        //while queue is not empty
        while(!q.isEmpty()){
            
            //check how many words are currently in the queue(this is also the number of words in the current "Level")
            int words = q.size();

            //loop through each word
            for(int k=0; k<words; k++){
                
                //get the current word and change it to char array (using str.substring() will lead to TLE)
                char[] currentWord = q.poll().toCharArray();

                //for each position of the current word, replace it with alphabets from a to z (except for the already existing character)
                //check if that forms a word which is valid (present in the HashSet)
                //if it's present in the HashSet, remove it from the HashSet (this will be the least number of transformations needed) and add it to the queue (so that we can check that word in the next iteration if needed)
                for(int pos=0; pos<currentWord.length; pos++){
                    
                    //save the current character which is at position pos
                    //we will need this to undo our changes
                    char tmp = currentWord[pos];
                    
                    //keep replacing the character at position "pos" in the current word
                    for(char c='a'; c<='z'; c++){

                        //skip if it's already matching
                        if(currentWord[pos] == c) continue;

                        currentWord[pos] = c;

                        //convert to string for lookup
                        String nextWord = new String(currentWord);

                        //if the formed word is valid
                        if(set.contains(nextWord)){

                            //add it to the queue so that we can check further in the next iteration
                            q.add(nextWord);

                            //remove it from the set since it has been visited
                            set.remove(nextWord);
                        }

                        //if the next possible word is already matching the end word, we can return the count of transformations
                        if(nextWord.equals(endWord)) return count+1;

                    }

                    //if endWord was not found, we will try moving to the next position
                    //before that, we need to restore the character at current position pos
                    currentWord[pos] = tmp;

                }

            }
            
            //increase the count of transformations needed as we move to the next level
            count++;

        }

        //end word could not be formed
        return 0;

    }

}
```

### APPROACH 2

The idea is to find and remove the next possible words which are possible from the current word we have at hand. This is similar to the idea of BFS, as we keep exploring the neighboring words at the next "level". This approach is not so optimal time wise, however, the problem contains up to 5000 words in the word list. So, this is an acceptable solution. In the worst case, we will match will only a single word in the list of words. Even for that we would be checking n-1 words. But for the next iteration, we can remove the words which have been visited as a small optimization. The final result is the number of steps it took to reach the endWord. If we did not encounter the endWord, we can return 0.

```java
class Solution {

    // Function to check if two words are valid transformations of each other
    public boolean valid(String a, String b){
        if(a.length() != b.length()) return false;
        int diff = 0;
        for(int i=0; i<a.length(); i++){
            if(a.charAt(i) != b.charAt(i)) diff++;
            if(diff >= 2) return false; // If more than 1 letter is different, not a valid transformation
        }

        // If at most 1 letter is different, they are valid transformations
        return true; 
    }
    
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {

        Set<String> possibleWords = new HashSet<>();
        for(String s: wordList) possibleWords.add(s); // Store the wordList in a HashSet for efficient lookup

        Queue<String> q = new LinkedList<>();
        q.offer(beginWord); // Start with the initial word

        int steps = 0; // To keep track of the transformation steps

        while(!q.isEmpty()){

            steps++; // Increment the step count

            int elements = q.size(); // Number of elements at this level of transformation

            for(int i=0; i<elements; i++){

                String currentWord = q.poll(); // Get the current word from the queue

                Set<String> nextPossibleWords = new HashSet<>(); // Set to maintain the list of next words which are possible from the currentWord

                for(String nextWord: wordList){

                    if(valid(currentWord, nextWord)){
                        
                        nextPossibleWords.add(nextWord); // Add potential next words to the set
                        q.add(nextWord); // Add potential next words to the queue
                        
                        if(nextWord.equals(endWord)) return steps + 1; // If the end word is reached, return the steps

                    }

                }

                // Remove visited words from the word list to avoid unnecessary checks (mark visited)
                wordList.removeAll(nextPossibleWords);

            }

        }

        // If no transformation sequence is found
        return 0; 

    }

}
```
