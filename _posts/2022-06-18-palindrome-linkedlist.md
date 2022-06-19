---
layout: post
title: Palindrome LinkedList
date: 2022-06-18 11:26 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "leetcode"
---

### PROBLEM DESCRIPTION

Given the head of a singly linked list, return true if it is a palindrome.  [Palindrome LinkedList](https://leetcode.com/problems/palindrome-linked-list/solution/)

### SOLUTION

### Using Stack (O(n) space complexity)

![snapshot]({{ site.baseurl }}/assets/img/palindrome_linked_list.png)

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        
        int length=0;
        ListNode temp=head;
        
        while(temp!=null){
            length++;
            temp=temp.next;
        }
        
        boolean skip = (length%2==1?true:false);
        
        Stack s = new Stack();
        
        temp=head;
        
        for(int i=0;i<length;i++){
            
            if(skip && i==(length/2)){
                temp=temp.next;
                continue;
            }
            
            if(i<(length/2)){
                s.push(temp.val);
            }else{
                
                if(!s.empty()){
                    if((int)s.peek() == temp.val){
                        s.pop();
                    }else{
                        return false;
                    }
                }
                
            }
            
            temp = temp.next;
        
        }
        
        return s.empty();
        
    }
}
```

### Optimized Approach (O(1) space complexity)

- Reverse 2nd half of the LinkedList
- Compare and check if elements match
- Reverse it back again so that we don't modify the given LinkedList
