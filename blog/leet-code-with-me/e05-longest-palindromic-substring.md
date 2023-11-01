# Episode 05 - Longest Palindromic Substring

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string `s`, return the longest palindromic substring in `s`.

Example: 

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

## Suboptimal solution

The brute force approach is to generate all the substrings and validate each of them. 

```java
class Solution {
    public String longestPalindrome(String s) {
        String result = "";
        for (int i = 0; i < s.length(); i++) {
            StringBuilder candidate = new StringBuilder();
            for (int j = i; j < s.length(); j++) {
                candidate.append(s.charAt(j));
                if (isPalindromic(candidate)) {
                    if (candidate.length() > result.length()) {
                        result = candidate.toString();
                    }
                }
            }
        }
        return result;
    }
    
    private boolean isPalindromic(StringBuilder s) {
        boolean result = true;
        if (s.length() % 2 == 0) {
            for (int i = 0; i < (s.length() / 2); i++) {
                if (s.charAt(i) != s.charAt(s.length() - i - 1)) {
                    result = false;
                    break;
                }
            }
        } else {
            for (int i = 0; i < ((s.length() - 1) / 2); i++) {
                if (s.charAt(i) != s.charAt(s.length() - i - 1)) {
                    result = false;
                    break;
                }
            }
        }
        return result;
    }
}
```

The solution works but does not show expected performance. 

![Not good](./images/e05-01.png)

## Optimal solution

You may notice that the task is really similar to the [longest substring without repeating characters](./e03-longest-substring-without-repeating-characters.md). That task was solved using a sliding window approach and we may apply it here as well. 

```java
class Solution {
    public String longestPalindrome(String s) {
        String result = "";
        for (int i = 0; i < s.length(); i++) {
            int left = i; 
            int right = i; 

            while (right < s.length() - 1 && s.charAt(left) == s.charAt(right + 1)) {
                right++;
            }
            
            while (
                left > 0 && right < s.length() - 1 &&
                s.charAt(left - 1) == s.charAt(right + 1)
            ) {
                left--;
                right++;
            }

            if (right - left + 1 > result.length()) {
                result = s.substring(left, right + 1);
            }
        }
        return result;
    }
}
```

This approach shows much better results. 

![Sliding window](./images/e05-02.png)