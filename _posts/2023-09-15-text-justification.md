---
layout: post
title: Text Justification
date: 2023-09-15 21:48 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an array of strings words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

Note:

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
- The input array words contains at least one word.

[leetcode](https://leetcode.com/problems/text-justification/)

## SOLUTION

[Good Explanation - Hindi](https://www.youtube.com/watch?v=jpU2LVaDa4g)

```java
class Solution {

    public List<String> fullJustify(String[] words, int maxWidth) {

        int n = words.length;

        //index i will be used to track the start of the line
        int i=0;

        List<String> lines = new ArrayList<>();

        //while we have more words to start from
        while(i<n){

            //we can definitely add the word at current starting position i
            //the next check will be from i+1 index onwards
            //so we use index j starting one position ahead
            int j = i+1;

            //init: since we can definitely include words at index i, let's add that
            int charCount = words[i].length();

            //number of gaps between words (if there are n words, the numbers of gaps will be n-1)
            //example: this_is_a_test_line has 5 words and 4 gaps
            //our intention will be to use a single space and pack as many words as possible for now
            int gaps = 0;

            //keep checking the next words and see if it can be included within maxWidth
            //Example: this is an example
            //i=0, j=1
            //can we take word[j]?
            //if current character count + number of gaps/spaces + current word length + 1 for extra space that will be added before word[j] can fit in maxWidth, we can include it
            while(j<n && charCount + gaps + words[j].length() + 1 <= maxWidth){
                charCount += words[j].length();
                gaps++; //we will need one gap while adding another word
                j++;    //go to next word
            }

            //now we know that the words which can be included in the current line are from index [i, j-1]
            //how many spaces are left? maxWidth - sum of characters for words from [i, j-1]
            int remainingSpaces = maxWidth - charCount;

            //how many spaces should be there for each gap?
            int spaceForGaps = gaps>0?remainingSpaces/gaps:0;

            //it's possible that there are some extra spaces left
            //Example: remainingSpaces = 5, gaps = 2
            //how many extra spaces are left?
            //as per the problem, we need to add these extra spaces from left to right
            int extraSpace = gaps>0?remainingSpaces%gaps:0;

            //when j == n, we must be in the last line
            //as per the problem, for the last line, there should be only 1 space between the words
            if(j == n){
                spaceForGaps = 1;
                extraSpace = 0;
            }

            //build the line based on the index and spaces calculated and add it to the list
            lines.add(buildLine(words, i, j, spaceForGaps, extraSpace, maxWidth));

            //for the next line, we should start from index j
            //note that words[j] was not added in the previous line because it did not fit within maxWidth
            i = j;

        }

        return lines;

    }

    public String buildLine(String[] words, int i, int j, int spaceForGaps, int extraSpace, int maxWidth){

        StringBuilder sb = new StringBuilder();

        //get the words from index [i, j-1]
        for(int k=i; k<j; k++){

            //add the word to the line
            sb.append(words[k]);

            //IMPORTANT: if it's the last word, don't add any extra space after that
            if(k == j-1) break;

            //add the required number of spaces calculated for each gap
            for(int s=0; s<spaceForGaps; s++){
                sb.append(" ");
            }

            //if there are extraSpaces left, add just 1 and reduce the count of it
            //as per the problem, we need to use this greedly from left to right only
            if(extraSpace > 0){
                sb.append(" ");
                extraSpace--;
            }

        }

        //IMPORTANT:
        //there can be a case in which we add only a single word
        //in that case, we will have to fill up other positions with empty space
        while(sb.length() < maxWidth){
            sb.append(" ");
        }

        return sb.toString();

    }

}
```
