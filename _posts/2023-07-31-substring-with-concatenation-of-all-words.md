---
layout: post
title: Substring with Concatenation of All Words
date: 2023-07-31 21:38 +0530
author: "Gaurav Kumar"
tags: "java sliding_window arrays leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

You are given a string s and an array of strings words. All the strings of words are of the same length.
A concatenated substring in s is a substring that contains all the strings of any permutation of words concatenated.
For example, if words = ["ab","cd","ef"], then "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", and "efcdab" are all concatenated strings. "acdbef" is not a concatenated substring because it is not the concatenation of any permutation of words.
Return the starting indices of all the concatenated substrings in s. You can return the answer in any order.

[leetcode](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

## SOLUTION

### APPROACH 1

The first approach is actually quite intuitive. We know the number of words in words[] array. Each word has the same length. So, a valid permutation substring in s will be of length: ```number of words x length of each word```. Also, note that the words in the words[] array can repeat. So, we will need to maintain a HashMap of frequency as well.

So, first we can create a HashMap of words and their frequency for the given words[] array.  
Then, iterate starting from each character in the string s. This will be the possible starting point. Since the length of the total string can be ```number of words x length of each word```, we can iterate till ```s.length()-totalLength```, which can be the final starting point of a valid substring.  

In the second loop, we can use another HashMap to keep a track of words which have been matched. The second loop will jump by "word length". The next word which needs to be checked will be: ```s.substring(j, j+wlen);```. If this is not present in the list of given words, we can simply break and continue with next index. If it is present, we should check how many times have been "seen" that word. If it was seen more than the number of times it was provided in the list of words, then we break. Otherwise, we can increase the count of words which have matched.  

If this count matches the number of words in the list of words, we have found an answer, which starts at index i. So, add this to the list.

```java
class Solution {
    
    public List<Integer> findSubstring(String s, String[] words) {
        
        int n = words.length;

        //number of words
        int wcount = words.length;

        //length of each word
        int wlen = words[0].length();

        //total length (the valid permutation substring should be of this length)
        int totalLength = wlen * wcount;

        //frequency of words in the given list of words
        //note: the words in the array can repeat
        Map<String, Integer> mapOfWords = getFrequencyMap(words);
        
        //answer list to return
        List<Integer> ans = new ArrayList<>();

        //check from each index upto s.length()-totalLength because we need at least "wlen * wcount" length for a valid answer
        for(int i=0; i<=s.length()-totalLength; i++){

            //how many word matched
            int match = 0;

            //init: words we have seen so far
            Map<String, Integer> seen = new HashMap<>();

            //start from ith index
            //jump by length of the word since each word has same length
            //do this until we have matched "wcount" words
            for(int j=i; match < wcount; j+=wlen){
                
                //next word to check
                String next = s.substring(j, j+wlen);

                //put it in the seen hashmap
                seen.put(next, seen.getOrDefault(next, 0) + 1);

                //if this next word is not valid, we can break
                //important: we should also break if this word was valid, BUT it was seen more than the number of times it's present in the ilst of words given
                if(!mapOfWords.containsKey(next) || seen.get(next) > mapOfWords.get(next)){
                    break;
                }

                //otherwise, we have found a match
                match++;

            }

            //if the number of words matched equals the number of words given, we have a valid answer
            if(match == wcount){
                ans.add(i);
            }


        }

        return ans;

    }

    public Map<String, Integer> getFrequencyMap(String[] words){

        Map<String, Integer> map = new HashMap<>();

        for(int i=0; i<words.length; i++){
            map.put( words[i], map.getOrDefault(words[i], 0) + 1);
        }

        return map;

    }


}
```

### APPROACH 2 (SLIDING WINDOW)

[good explanation](https://leetcode.com/problems/substring-with-concatenation-of-all-words/solutions/1753357/Clear-solution!-Easy-to-understand-with-diagrams%5C/)

1. **Sliding Window Initialization:**
   - Start with both `left`, `right` pointers at the beginning of the string (`i`).

2. **Sliding Window Expansion:**
   - Keep expanding the window on the right if the current word matches any word in the given `words` array.

3. **Mismatch Handling:**
   - If there is a mismatch for the next word on the right, reset the `left` and `right` pointers to the next word to continue searching.

4. **Match Handling:**
   - If the next word on the right is found in the `words` array, add it to the `seen` hashmap and increase its frequency.
   - If the frequency of the current word in the `seen` hashmap exceeds the frequency of the word in the `wordFreq` hashmap (i.e., it was seen more times than it's present in the given `words` array), reduce the window from the left.
   - Keep doing this until the frequency of the current word in the `seen` hashmap matches the frequency in the `wordFreq` hashmap.

5. **Concatenated Substring Found:**
   - If the number of matches found (`match`) is equal to the word count (`wcount`), it means all words in the `words` array are present in the current window as a concatenated substring.

6. **Add to Result:**
   - In that case, the index `left` must be one of the starting indices of the concatenated substring, so add it to the `ansList`.

```java
class Solution {
    
    public List<Integer> findSubstring(String s, String[] words) {

        int n = s.length();

        int wcount = words.length;
        int wlength = words[0].length();

        int totalLength = wcount * wlength;

        //frequency map of the given words
        Map<String, Integer> wordFreq = new HashMap<>();
        for(String x: words){
            wordFreq.put(x, wordFreq.getOrDefault(x, 0) + 1);
        }

        //answer
        List<Integer> ansList = new ArrayList<>();

        //there will be multiple sliding windows starting from 0, 1, 2...(wlen-1)
        for(int i=0; i<wlength; i++){
            
            //how many words have matched
            int match = 0;
            Map<String, Integer> seen = new HashMap<>();

            //sliding window starting from left=right=i
            //keeping expanding it on right if the words match
            //if there is a mismatch for the next word on the right, reset left and right to the next word
            //if the next word on the right is found in the list of given words then:
            //add it to the seen hashmap and increase it frequency
            //if it was seen more than the number of times it's present in the list of given words, try to reduce the windows from left
            //we will keep doing this until the frequency of the current word matches (in wordFreq HashMap and seen HashMap)
            //finally, check if the number of matches found is equal to the word count
            //in that case, index left must be one of the answers
            for(int left=i, right=i; right <= n-wlength; right += wlength){

                String next = s.substring(right, right+wlength);

                if(!wordFreq.containsKey(next)){
                    match = 0;
                    seen.clear();
                    left = right + wlength; //right will also become same due to for loop
                }else{

                    seen.put(next, seen.getOrDefault(next, 0) + 1);

                    //reduce window from left until the frequency becomes the same
                    while(seen.get(next) > wordFreq.get(next)){

                        String leftString = s.substring(left, left+wlength);
                        seen.put(leftString, seen.get(leftString)-1);

                        match--;
                        left += wlength;
                    }

                    match++;

                }

                if(match == wcount){
                    ansList.add(left);
                }


            }

        }

        return ansList;

    }

}
```
