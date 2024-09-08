---
layout: post
title: Special Keyboard (geeksforgeeks - SDE Sheet)
date: 2024-09-08 16:43 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Imagine you have a special keyboard with the following keys:

- Key 1: Prints 'A' on screen
- Key 2: (Ctrl-A): Select screen
- Key 3: (Ctrl-C): Copy selection to buffer
- Key 4: (Ctrl-V): Print buffer on screen appending it after what has already been printed.

Find maximum numbers of A's that can be produced by pressing keys on the special keyboard N times.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/special-keyboard3018/1?page=5)

## SOLUTION

We have mainly two cases:

- At each step i, one option is to press key 1 to add a single 'A' to the screen. If we're at step i, we can get one more 'A' than what we had at step i-1.

- In this case, we look at the possibility of pressing Ctrl-A + Ctrl-C at a previous position `j` and then pasting the copied 'A's multiple times after that. The key is finding the right `j` where you would perform the Ctrl-A + Ctrl-C combination, which gives us the highest return by allowing us to paste the copied buffer multiple times.

  - `j` must be at least `i-3` to allow for:
  - Ctrl-A (1 keypress)
  - Ctrl-C (1 keypress)
  - At least one Ctrl-V (1 keypress)

  Now, for any given `j`, after copying the screen (Ctrl-A + Ctrl-C), the remaining key presses (from `j+2` to `i`) can be used for pasting (Ctrl-V). The number of pastes we can make is `(i - j - 2)`.

  The total number of 'A's produced from this operation is:

  `dp[j] + dp[j] * (i - j - 2)`

```java
class Solution {

    static int optimalKeys(int n) {

        // If the number of key presses is 3 or less, the maximum number of 'A's
        // that can be printed is equal to the number of key presses since no combinations
        // of Ctrl-A, Ctrl-C, Ctrl-V would be beneficial.
        if (n <= 3)
            return n;

        // Array `dp` will store the maximum number of 'A's that can be produced
        // with i keystrokes, where dp[i] is the maximum result for i keystrokes.
        int[] dp = new int[n + 1];

        // Initialize the base cases:
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;

        // Loop through all keystrokes from 4 to n.
        for (int i = 4; i <= n; i++) {

            // Option 1: Add one 'A' with a simple key press.
            int o1 = dp[i - 1] + 1;

            // Option 2: Use Ctrl-A, Ctrl-C, and then multiple Ctrl-Vs to paste the buffer.
            // This can be done multiple times before the current position
            // So, we can go to each position before current position - 3, and check how many characters we can get if we had copied the elements at that position
            int o2 = 0;

            // j represents the position where we would press Ctrl-A, Ctrl-C after producing dp[j] 'A's.
            // (i-j-2) gives the number of Ctrl-V operations we can do after the copy.
            // Loop through all possible positions to decide when to start copying.
            for (int j = i - 3; j >= 1; j--) {
                // The formula dp[j] + dp[j] * (i - j - 2) represents:
                // dp[j]: The number of 'A's produced after j keystrokes.
                // (i - j - 2): The number of times Ctrl-V is used to paste the buffer.
                // dp[j] * (i - j - 2) adds the result of pasting the copied buffer multiple times.
                o2 = Math.max(o2, dp[j] + dp[j] * (i - j - 2));
            }

            // Set dp[i] as the maximum of either adding one more 'A' or using the copy-paste strategy.
            dp[i] = Math.max(o1, o2);
        }

        return dp[n];
    }
}
```
