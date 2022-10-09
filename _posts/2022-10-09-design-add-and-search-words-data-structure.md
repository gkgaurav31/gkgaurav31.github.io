---
layout: post
title: Design Add and Search Words Data Structure
date: 2022-10-09 19:47 +0530
author: "Gaurav Kumar"
tags: "java neet150"
categories: "neet150"
---

This question is part of [Neet150 series](https://neetcode.io/practice).  

## Problem Description

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:

- WordDictionary() Initializes the object.
- void addWord(word) Adds word to the data structure, it can be matched later.
- bool search(word) Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.

[leetcode](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

### Solution

This problem is a bit tricky because we have a "." character which can match will any character present in the Trie. To solve this problem, whenever we get a ".", we will need to loop through all the character possible in the next step and recursively look for the remaining substring. Interestingly, this problem lead to TLE if you use HashMap in the Node structure, but works with array[26].

```java
class Node{
    boolean isEnd;
    Node[] alphabets = new Node[26];
}

class WordDictionary {

    Node root;
    
    public WordDictionary() {
        root = new Node();
    }
    
    //Add word to the given Trie
    public void addWord(String word) {
        
        Node temp = root;
        
        for(int i=0; i<word.length(); i++){
            
            Character c = Character.valueOf(word.charAt(i));
            
            //If the Node for current alphabet is not present, create it and move to the next Node
            if(temp.alphabets[c-'a'] == null){
                temp.alphabets[c-'a'] = new Node();
                temp = temp.alphabets[c-'a'];
            }else{
                temp = temp.alphabets[c-'a'];
            }
            
        }
        
        temp.isEnd = true;
        
    }
    
    public boolean search(String word) {
        return searchHelper(word, root);  
    }
    
        
    public boolean searchHelper(String word, Node root){
        
        Node temp = root;
        
        //Loop through the characters of the given word
        for(int i=0; i<word.length(); i++){
            
            //Get the current character
            Character c = Character.valueOf(word.charAt(i));
            
            //If the current char is not a dot and it's pointing to null, return false.
            if(c != '.' && temp.alphabets[c-'a'] == null){
                return false;
            //If the current char is not a dot, but it has a reference Node, go to the child node.
            }else if(c != '.' && temp.alphabets[c-'a'] != null){
                temp = temp.alphabets[c-'a'];
            //If the current char is a dot
            }else if(c == '.'){
                
                //Loop through all the alphabets
                for(int j=0; j<temp.alphabets.length; j++){
                    
                    //Get the node corresponding to the character at temp.alphabets[index]
                    Node child = temp.alphabets[j];
                    
                    //If the next node is null, we do not have any mapping for the dot character, to continue for other characters
                    if(child == null) continue;
                    
                    //If the next node is not null, this is a possible path following the dot
                    //To check if rest of the string after dot is present, we can simply call the search method passing the remaining substring and the child node
                    if(searchHelper(word.substring(i+1), child)){
                        return true;
                    }
                    
                }

                //If no path was found any any of the children, return false
                return false;
            }
        
        }
        
        //Finally, we return temp.isEnd which will tell us if the word was actually added before
        return temp.isEnd;
    }    

    
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```
