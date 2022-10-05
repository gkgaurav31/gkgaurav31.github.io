---
layout: post
title: Trie - Introduction - Part 2
date: 2022-10-05 17:38 +0530
tags: "java trie"
categories: "trie"
---

### INTRODUCTION TO TRIE - PART 2

[Trie Introduction: Part 1]({% post_url 2022-10-05-trie-introduction %})

![snapshot]({{ site.baseurl }}/assets/img/trie_2.png)

### Implementation

[leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/)

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
        
        //Loop through the string
        for(int i=0; i<n; i++){
            
            //Get the current character
            Character c = Character.valueOf(word.charAt(i));
            
            //Check if the current Node's hashmap contains that character
            if(temp.hm.containsKey(c)){
                //If it contains the character already, go to the next Node. 
                //We need to go to the corresponding Node for the current character which we can get from the HashMap.
                temp = temp.hm.get(c);
            }else{
                //If the character is not present in the HashMap, new a new Node which will act like the next one for the current character.
                temp.hm.put(c, new Node(new HashMap<>()));
                //Once that's is set, go to the next node
                temp = temp.hm.get(c);
            }
            
        }
        
        temp.isEnd = true;
        
    }
    
    public boolean search(String word) {
        
        int n = word.length();
        
        Node temp = root;
        
        //Loop through the word
        for(int i=0; i<n; i++){
            
            //Get the current character
            Character c = Character.valueOf(word.charAt(i));
            
            if(!temp.hm.containsKey(c)){
                return false;
            }else{
                temp = temp.hm.get(c);
            }
            
        }
        //Since we need a complete match and not just prefix, we need to check if isEnd on the current node is true.
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
```
