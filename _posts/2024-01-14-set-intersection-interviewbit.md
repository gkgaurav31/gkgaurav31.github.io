---
layout: post
title: Set Intersection (InterviewBit)
date: 2024-01-14 14:19 +0530
author: "Gaurav Kumar"
tags: "java interviewbit leetcode arrays important"
categories: "interviewbit"
---

## Problem Description

An integer interval `[X, Y]` (for integers `X < Y`) is a set of all consecutive integers from X to Y, including X and Y.

You are given a 2D array A with dimensions N x 2, where each row denotes an interval.

Find the minimum size of a set S such that for every integer interval Z in A, the intersection of S with Z has a size of at least two.

[InterviewBit](https://www.interviewbit.com/problems/set-intersection/)
[leetcode](https://leetcode.com/problems/set-intersection-size-at-least-two/description/)

### Solution

This problem is actually quite difficult and there are not a lot of good explanations for it. I will try my best to explain with some examples.

We first sort the intervals in ascending order by their end value. The idea is to maintain a maximum and second maximum value which is the farthest to start with, expecting an overlap with the next intervals. Since we have already sorted in ascending order by end value, if we take the smallest values from previous interval, that does not help (less chance of overlap). Like: `[3, 7][5, 10]` If we taken {3,4} in the set, it wouldn't help with the next range. But, if we took `{7, 7-1}` = `{7, 6}`, since we have sorted in desc order by end value, it helps in getting more overlap.

If the end values are equal, sort by start values in descending value (Honestly, I am not sure why this is exactly done, but I think the idea is the same to push the largest ranges towards the right).

Now, mainly there are three conditions, we would need to include either 0 (at least 2 overlaps) or 1 (only one overlap) or 2 (no overlap) values to our set to keep the size of that set minimum. Let's try to understand this with some examples:

- `[1,3][2,7]`
  max = 3, secondMax = 2 (our set could be: {2,3})

  both `max` and `secondMax` are >= `nextStart` (2). So, both overlap.  
  We can use the existing set `{2,3}` and nothing changes.

  ```java
  if(maxEnd >= nextStart && secondMaxEnd >= nextStart){
      continue;
  }
  ```

- `[1,3][3,7]`
  `max` = 3, `secondMax` = 2 (our set could be: `{2,3}`)

  `max` is >= `nextStart` but `secondMax` is not.

  The best we can do it to include only 1 extra element so minimize the set size.
  Obviously, we should include the overlapped one.

  So should also try to increase our max values using the next range here. Previously we had `{2,3}`. 3 is overlapped. We need to add one extra element which will increase the set size by 1. We should choose the largest one which is 7. So the set becomes {2,3,7}. We are being greedy here as well, selecting 7 increases the chance of having overlap with next interval since we sorted the intervals in ascending oder by end value. Ensure that we also update `secondMax = max` first and then `max = nextEnd`.

  ```java
  if(maxEnd >= nextStart){
    secondMaxEnd = maxEnd;
    maxEnd = nextEnd;
    setSize += 1;
  }
  ```

- If the above conditions did not meet, that means both `max` and `secondMax` have lower values then `nextStart`. So, we did not get any overlap. This means, we will need two extra elements to be added to the set. We can increase the size of the set by 2. What should be the new max and second max? You guessed it right, we will again follow greedy approach and use the next interval's `nextEnd` and `nextEnd-1` as our `max` and `secondMax`!

  ```java
  else{
    maxEnd = nextEnd;
    secondMaxEnd = nextEnd - 1;
    setSize += 2;
  }
  ```

COMPLETE CODE:

```java
public class Solution {

    public int setIntersection(ArrayList<ArrayList<Integer>> A) {

        int n = A.size();

        // sort based on ascending order by end values
        // if end value is same, sort based on start value
        // the idea is to keep the range as max as possible from the beginning
        Collections.sort(A, (a,b) -> a.get(1) == b.get(1) ? b.get(0) - a.get(0) : a.get(1) - b.get(1));

        // init:
        // the min set size will be 2 for a given range [x,y] since (x < y) and we can take any two elements from that range
        // we are taking a greedy approach here, by taking the largest values in the set
        // example: [5, 10]
        // we can actually take {5,6} for a valid answer, but it might not be a good idea
        // we have sorted the array in ascending order by end value
        // so if we take the highest values in the current range, there is a larger chance of having an overlap
        // so we should consinder {9,10} -> {secondMaxEnd, maxEnd}
        int maxEnd = A.get(0).get(1);
        int secondMaxEnd = A.get(0).get(1) - 1;
        int setSize = 2;

        // interated from the next range in the list
        for(int i=1; i<n; i++){

            int nextStart = A.get(i).get(0);
            int nextEnd = A.get(i).get(1);

            // example: [1,3][1,4]
            // maxEnd = 3, secondMaxEnd = 2
            // we sorted based on the end value (ascending)
            // the next interval's end must be >= previous one; 4 >= 3
            // maxEnd and secondMaxEnd are the highest values we could have taken for previous range (greedy)
            // if this maxEnd and secondMaxEnd are part of next range (maxEnd >= currentStart && secondMaxEnd >= currentStart)
            // we can use the same maxEnd and secondMaxEnd in the set to have min size, without including any other element in the set
            // that set, for this example will be {2, 3} -> {secondMaxEnd, maxEnd}
            if(maxEnd >= nextStart && secondMaxEnd >= nextStart){
                continue;
            }

            // example: [1,4][4,5]
            // maxEnd = 4, secondMaxEnd = 3
            // maxEnd (4) >= nextStart (4) but secondMaxEnd (3) is not >= nextStart (4) -> previous condition
            // let's check if at least maxEnd is >= nextStart
            // so there is 1 element overlapped, which we can re-use
            // so, we just need to add more extra element to the set (so increase the size by 1)
            // what happens to the maxEnd and secondMaxEnd now?
            // we need to keep the overlapped element and extend the range
            // new maxEnd = 5
            // new secondMaxEnd = 4 (overlapped)

            // interesting example:
            // {secondMax, maxEnd} : {12, 14}  -> {nextStart, nextEnd} : {13, 15}
            // observe that secondMaxEnd will not be always one lower than maxEnd
            // you can take this sample to check:
            // [[4, 6], [2, 10], [6, 12], [12, 14], [11, 14], [13, 15], [3, 19], [8, 20], [7, 20]]
            if(maxEnd >= nextStart){
                secondMaxEnd = maxEnd;
                maxEnd = nextEnd;
                setSize += 1;

            // example: [1,4][5,6]
            // maxEnd = 4, secondMaxEnd = 3
            // we cannot use any existing element (no overlap)
            // so set size will have to be incresed by 2
            // to extend the max range, we will use the new max range
            }else{
                maxEnd = nextEnd;
                secondMaxEnd = nextEnd - 1;
                setSize += 2;
            }


        }

        return setSize;

    }

}
```
