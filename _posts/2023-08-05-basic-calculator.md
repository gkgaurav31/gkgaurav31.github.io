---
layout: post
title: Basic Calculator
date: 2023-08-05 12:30 +0530
author: "Gaurav Kumar"
tags: "java stack leetcode leetcode150 important"
categories: "stack"
---

## PROBLEM DESCRIPTION

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.
Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().

[leetcode](https://leetcode.com/problems/basic-calculator/)

## SOLUTION

[leetcode editorial](https://leetcode.com/problems/basic-calculator/editorial/)

Example:

> (7-8+9)

Let's add to stack from left to right:

```text
Stack:
)
9
+
8
-
7
(
```

This will evaluate to:

```text
Stack:
)
17
-
7
(
```

This will evaluate to:

```text
Stack:
)
10
(
```

This is a WRONG ANSWER!

This happens because unlike addition, subtraction is not commutative or associative. If we use a stack and read the elements of the expression from left to right, we end up evaluating the expression from right-to-left. This means we are expecting (A−B)−C(A-B)-C(A−B)−C to be equal to (C−B)−A(C-B)-A(C−B)−A which is not correct.

To handle this, we can go from right to left instead.  

The stack will look like this:

> (7-8+9)

```text
(
7
-
8
+
9
)
```

Which evaluates to:

```text
(
-1
+
9
)
```

Which evaluates to:

```text
(
8
)
```

```java
class Solution {
    
    public int evaluateCurrentState(Stack<Object> stack){

        //if stack is empty or the top most element is not an integer, then add 0 to make it simpler to calculate
        if(stack.isEmpty() || !(stack.peek() instanceof Integer)){
            stack.push(0);
        }

        //init: start the calculation using top most element (which is why we had added 0 earlier to make this part simpler)
        //if the top was already a digit, that's alright too
        int res = (int) stack.pop();

        //it's possible that there are no brackets and stack can become empty, so check if stack is not empty
        //we need to evaluate till the corresponding closing bracket
        while(!stack.isEmpty() && (char)stack.peek() != ')'){
            
            //next character must be the sign 
            char sign = (char) stack.pop();

            //calculate based on sign
            //the next popped number will be the next digit
            if(sign == '+'){
                res += (int) stack.pop();
            }else{
                res -= (int) stack.pop();
            }

        }

        return res;

    }

    public int calculate(String s) {
        
        //create a stack of Object type because it needs to have both integers and characters for operations/signs 
        Stack<Object> stack = new Stack<>();

        //we will form the operand which needs to be added or subtracted while iterating through the string from right to left
        //example: 1 + 123
        //since we are iterating from right, first we meet 3
        //operand += 3 * (10 power 0)
        //increment the power (p) for the next digit
        //but we don't add the number to the stack yet
        //next digit is 2. 
        //operand += 2 * (10 power 1), because p is now 1
        //similary, operand += 1 * (10 power 2)
        //we have formed the operand 123, but not yet added it to the stack
        //we will add it when we encounter a non-integer
        int operand = 0;
        int p = 0; //power

        //loop from right to left
        for(int i=s.length()-1; i>=0; i--){

            //get current character
            char c = s.charAt(i);

            //if current character is a digit
            if(Character.isDigit(c)){
                
                //add to the operand and increment the power value for the next digit
                operand += (int)Math.pow(10, p) * (int)(c-'0');
                p++;

            //if the next character is not a space
            }else if(c != ' '){
                
                //it's possible that we had formed some operand which is yet to be added
                //this can be checked by checking the value of power p
                //if this is not 0, it means, there must be some operand and we can add it to the stack
                //remember to reset the value of operand and power for subsequent numbers which might come up
                if(p != 0){
                    stack.push(operand);
                    operand = 0;
                    p = 0;
                }

                //if we encounter an opening bracket, we have found a pair and we can calculate the result of this sub-expression
                if(c == '('){
                    
                    //calculate value for the subexpression
                    int res = evaluateCurrentState(stack);

                    //pop the opening bracket (remember, we are going in reverse)
                    stack.pop();

                    //push the current result to the stack
                    //this may be used for subsequent evaluation
                    stack.push(res);

                }else{

                    //otherwise, push to stack
                    stack.push(c);
                }

            }

        }

        //if power is not 0, we have some operand which is yet to be added to the stack
        //example expression: 1234 -> this will form the operand but never get added in the for-loop
        if(p != 0){
            stack.push(operand); 
        }

        //evaluate any left overs in the stack and return the final answer
        return evaluateCurrentState(stack);

    }

}
```
