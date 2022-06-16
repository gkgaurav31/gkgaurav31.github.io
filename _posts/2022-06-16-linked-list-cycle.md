---
layout: post
title: Linked List Cycle
date: 2022-06-16 16:11 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "leetcode"
---

### PROBLEM DESCRIPTION: [Linked List Cycle - Find Start of Cycle](https://leetcode.com/problems/linked-list-cycle-ii/)

If there is a cycle in the LinkedList, find the Node where the cycle starts.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/linked_list_cycle.jpg)

We can easily find if the cycle exists by using slow and fast pointers. If they meet, we know that there is a cycle in the list. From the intersection point in the cycle, we need to do the following:

- Move fast node by 1
- Reset slow node to head
- Move both fast and slow one step at a time until both are same
- Return the node which is same (entrypoint of the cycle)

### Intuition

Let's say:  
x1 = length from start to endtrypoint of cycle  
x2 = length from entrypoint to intersection of slow and fast  
x3 = length from intersection of slow and fast to entrypoint  
[Refer to the above picture]

We can say that:  
The length travelled by fast is 2 times the length travelled by slow.  

> => x1 + x2 + x3 + x2 = 2(x1+x2) ->> Important note: This is when ignoring multiple cycles by fast in certain scenarios  
> => x1 = x3  

[REFERENCE](https://leetcode.com/problems/linked-list-cycle-ii/discuss/1701055/JavaC%2B%2BPython-best-explanation-ever-happen's-for-this-problem)

```java
public class Solution {
    
    public ListNode detectCycle(ListNode head) {
        
        if(head == null) return null;

        ListNode slow = head;
        ListNode fast = head.next;

        while(fast != null && fast.next != null){

            slow = slow.next;
            fast = fast.next.next;

            //If slow==fast, cycle exists. We have found the intersection point.
            if(slow == fast){
                
                //We move the fast node one step further and reset the slow node to head. Then we keep moving them one step at a time until they meet. This will be our answer - start Node of the cycle
                fast = fast.next;
                slow = head;
                
                while(slow != fast){
                    slow = slow.next;
                    fast = fast.next;
                }                
                
                return slow;
                
            }

        }

        return null;  

        
    }
}
```

