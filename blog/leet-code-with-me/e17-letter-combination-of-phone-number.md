# Episode 17 - Letter Combinations of a Phone Number

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

Example: 

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

## The solution

Obviously, the mapping itself is required but the main idea of the solution is that it is recursive - start with the first gigit from the `digits` string we retrieve all the possible letters, and one by one add it to the `builder`. 

For example, if `digits` is `23`, then the first iteration would be: 

| index | builder |
|-------|---------|
| 0     | `""`    |

Then iterate over `a`, `b` and `c`, for example, the first iteration: 

| index | builder |
|-------|---------|
| 0     | `"a"`   |

After than, moves `index` to the next digit from the `digits` string, and repeat the operation for `d`, `e` and `f`. 

| index | builder |
|-------|---------|
| 0     | `"a"`   |
| 1     | `"ad"`  |

And so on. When a particular combination is checked, it is necessary to remove the last character from the `builder` and start next iteration of the loop. 

When all the digits are processed (`index >= digits.length()`), add the final string to the `result`. 

```java
class Solution {
    private final Map<Character, List<Character>> mapping = Map.of(
        '2', List.of('a', 'b', 'c'), 
        '3', List.of('d', 'e', 'f'),
        '4', List.of('g', 'h', 'i'),
        '5', List.of('j', 'k', 'l'),
        '6', List.of('m', 'n', 'o'),
        '7', List.of('p', 'q', 'r', 's'), 
        '8', List.of('t', 'u', 'v'), 
        '9', List.of('w', 'x', 'y', 'z')
    );

    private final List<String> result = new ArrayList();
    private final StringBuilder builder = new StringBuilder();

    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return List.of();
        }
        candidates(digits, 0);
        return result; 
    }

    private void candidates(final String digits, final int start) {
        if (start < digits.length()) {
            final List<Character> chars = mapping.get(digits.charAt(start));
            for (char c : chars) {
                builder.append(c);
                candidates(digits, start + 1);
                builder.deleteCharAt(start);
            }
        } else {
            result.add(builder.toString());
        }
    }
}
```