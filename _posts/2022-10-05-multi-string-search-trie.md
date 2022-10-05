---
layout: post
title: Multi String Search (Trie)
date: 2022-10-05 19:54 +0530
author: "Gaurav Kumar"
tags: "java binarytree"
categories: "binarytree"
---

### PROBLEM DESCRIPTION

You are given a Big String and an array of small strings.
You need to check whether each small string is present in the big string. Do not use any in-built method.

### SOLUTION

We can make use of Trie to solve this problem. We loop through the bigstring and keep adding the subarrays [0,n-1] [1,n-1], [2,n-1] and so on. Then, all we need is to check whether the smallstring is present in the Trie as a prefix.  

```java
import java.util.*;

class Program {
  
  public static List<Boolean> multiStringSearch(String bigString, String[] smallStrings) {

        Trie trie = new Trie();
        for(int i=0; i<bigString.length(); i++){
            trie.insert(bigString.substring(i));
        }

        List<Boolean> list = new ArrayList<>();
        for(int i=0; i<smallStrings.length; i++){
            list.add(i, trie.startsWith(smallStrings[i]));
        }

        return list;
    
  }
}

class Trie {
    
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
    
    //Return true if complete match is found
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
    
    //Returns true is the input string is found (can be a prefix) 
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
```