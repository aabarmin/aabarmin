# Episode 3 - Longest Substring without repeating characters

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string s, find the length of the longest substring without repeating characters.

## The solution

This is a sliding window problem. Overall approach to solving such kind of challenges it to have two pointers - one points to the left side of the window, another points to the right side of the window. Also it is necessary to define a condition that checks that the value of the window, for example, all the elements are in the ascending order, all the elements are number, etc. 

While the condition is met, it is necessary to move right pointer to the end of the initial collection. If the condition is not longer met, it is necessary to move the left pointer forward. 

For this exact challenge we also introduce a `seen` map that allows us to track positions of all the seen characters so that it will be easier to check if there any repetitions in the window. 

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int max = 0;
        
        int left = 0; 
        int right = 0; 
        int len = s.length();

        final Map<Character, Integer> seen = new HashMap<>();
        while (right < len) {
            final char nextChar = s.charAt(right);
            if (seen.containsKey(nextChar) && seen.get(nextChar) >= left) {
                left = seen.get(nextChar) + 1;
            } else {
                max = Math.max(max, right - left + 1);
            }
            seen.put(nextChar, right);
            right++;
        }

        return max; 
    }
}
```