---
layout: post
title: Implement Trie (Prefix Tree)
date: 2022-10-09 16:20 +0530
author: "Gaurav Kumar"
tags: "java neet150"
categories: "neet150"
---

This question is part of [Neet150 series](https://neetcode.io/practice).  

## Problem Description

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

Trie() Initializes the trie object.
void insert(String word) Inserts the string word into the trie.
boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

[leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/)

### Solution

```java
public class Trie {
    
    Node root;
    
    class Node{
        boolean isEnd;
        Map<Character, Node> hm;
        Node(Map<Character, Node> hm){
            this.hm = hm;
        }
    }
    
    public Trie() {
        root = new Node(new HashMap<>());
    }
    
    public void insert(String word) {
        
        int n = word.length();
        
        Node temp = root;
        
        for(int i=0; i<n; i++){
            
            Character c = Character.valueOf(word.charAt(i));
            
            if(temp.hm.containsKey(c)){
                temp = temp.hm.get(c);
            }else{
                temp.hm.put(c, new Node(new HashMap<>()));
                temp = temp.hm.get(c);
            }
            
            
        }
        
        temp.isEnd = true;
        
    }
    
    public boolean search(String word) {
        
        int n = word.length();
        
        Node temp = root;
        
        for(int i=0; i<n; i++){
            
            Character c = Character.valueOf(word.charAt(i));
            
            if(!temp.hm.containsKey(c)){
                return false;
            }else{
                temp = temp.hm.get(c);
            }
            
        }
        
        return temp.isEnd;

    }
    
    public boolean startsWith(String prefix) {
        int n = prefix.length();
        
        Node temp = root;
        
        for(int i=0; i<n; i++){
            
            Character c = Character.valueOf(prefix.charAt(i));
            
            if(!temp.hm.containsKey(c)){
                return false;
            }else{
                temp = temp.hm.get(c);
            }
            
        }
        
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```
