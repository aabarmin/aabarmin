# Episode - Template

⬅️ [Back to index](README.md)

## The task

Given a string s, find the length of the longest substring without repeating characters.

## The solution

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