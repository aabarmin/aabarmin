# Episode 12 - Integer to Roman

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

For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral.

Example: 

```
Input: num = 3
Output: "III"
Explanation: 3 is represented as 3 ones.
```

## Simple solution

Roman is not a positional numeral system so that it works slightly different than our ordinary decimal numeral system. For example, we have number 3456 and it has three 1000, four 100, five 10 and six. Next we need to convert them to letters one by one: 

* three 1000 is `MMM`,
* four 100 is `CD` because three 100s is `CCC`, five 100 is `D` but four follows the same rule as `IV` so that it is `CD`, 
* five 10 is `L` because `L` is 50 (not `XXXXX` obviously)
* and six is `VI`. 

In common the logic is if the number is close to the next letter - subtract rather than add. So that the easiest option is to utilize this bug in the numeral system and prepare values for all the numbers. 

```java
class Solution {
    public String intToRoman(int num) {
        String M[] = {"", "M", "MM", "MMM"};
        String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        return M[num / 1000] + C[(num % 1000) / 100] + X[(num % 100) / 10] + I[num % 10];        
    }
}
```

The solution is really simple but has these complicated `(num % 1000) / 100` operations. 

## Better solution

Another option is to use `TreeMap`'s function `floorKey` - the function return the greatest key that is less or equal from the parameter. For example, if we have keys `10`, `20`, and `30`, the parameter is `25`, the `floorKey` function will return key equal to `30`. By using this approach we can avoid complicated computations. 

```java
class Solution {
    private static final TreeMap<Integer, String> PARTS = new TreeMap<>();

    static {
        PARTS.put(1, "I");
        PARTS.put(4, "IV");
        PARTS.put(5, "V");
        PARTS.put(9, "IX");
        PARTS.put(10, "X");
        PARTS.put(40, "XL");
        PARTS.put(50, "L");
        PARTS.put(90, "XC");
        PARTS.put(100, "C");
        PARTS.put(400, "CD");
        PARTS.put(500, "D");
        PARTS.put(900, "CM");
        PARTS.put(1000, "M");
    }
    
    public String intToRoman(int num) {
        final int key = PARTS.floorKey(num);
        if (key == num) {
            return PARTS.get(key);
        }
        return PARTS.get(key) + intToRoman(num - key);
    }
}
```