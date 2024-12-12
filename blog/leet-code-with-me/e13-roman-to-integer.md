# Episode 13 - Roman to Integer

⬅️ [Back to index](README.md)

## The task

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

## The solution

Keeping in mind knowledge from the previous task [integer to roman](./e12-integer-to-roman.md) we can do reverse operation: 

```java
class Solution {
    private static final Map<Character, Integer> PARTS = new HashMap<>();

    static {
        PARTS.put('I', 1);
        PARTS.put('V', 5);
        PARTS.put('X', 10);
        PARTS.put('L', 50);
        PARTS.put('C', 100);
        PARTS.put('D', 500);
        PARTS.put('M', 1000);
    }

    public int romanToInt(String s) {
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            int current = PARTS.get(s.charAt(i));
            int next = i < s.length() - 1 ? PARTS.get(s.charAt(i + 1)) : 0;
            if (next > current) {
                current = next - current; 
                i = i + 1;
            }
            result = result + current; 
        } 
        return result; 
    }
}
```