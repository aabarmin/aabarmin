# Episode 22 - Generate Parentheses

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/generate-parentheses/description/)

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

## Suboptimal solution

The main idea is to use a backtracking approach - perform the operation until the expected result is found. The main complexity with backtracking is that it is necessary to perform two operations: 

1. An operation tha generates an initial state. 
2. A condition that says that the final result is reached. 
3. An operation that generates the next candidate. 

In this task we need to generate a string with brackets so that the initial state is an empty string `""`, number of opened and closed brackets is `0`. The condition to check at every iteration is length of the string - it should be twice longer than `n`. There is no need to check if brackets are paired because we always generate valid pairs. 

The last one part is about generation of brackets - if number of opened brackets is less than `n` - it is necessary to generate a candidate with one more opened bracket. If number of closing brackets is less than number of opened brackets - generate one more closing bracket. 

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        final List<String> result = new ArrayList<>();
        process(result, "", 0, 0, n);
        return result; 
    }

    private void process(
        final List<String> result, 
        final String currentString, 
        final int opened, 
        final int closed, 
        final int max
    ) {
        if (currentString.length() == max * 2) {
            result.add(currentString); 
            return; 
        }
        if (max > opened) {
            process(result, currentString + "(", opened + 1, closed, max);
        } 
        if (opened > closed) {
            process(result, currentString + ")", opened, closed + 1, max);
        }
    }
}
```

The solution is good but not perfect. 

![Not perfect](./images/e22-01.png)

## Optimal solution

There is nothing to improve from the algorithm point of view but in Java strings are immutable so every time we do `currentString + "("` a new string is created. What if we replace `String` with `StringBuilder`? 

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        final List<String> result = new LinkedList<>();
        process(result, new StringBuilder(), 0, 0, n);
        return result; 
    }

    private void process(
        final List<String> result, 
        final StringBuilder builder, 
        final int opened, 
        final int closed, 
        final int max
    ) {
        if (builder.length() == max * 2) {
            result.add(builder.toString()); 
            return; 
        }
        if (max > opened) {
            builder.append("(");
            process(result, builder, opened + 1, closed, max);
            builder.deleteCharAt(builder.length() - 1);
        } 
        if (opened > closed) {
            builder.append(")");
            process(result, builder, opened, closed + 1, max);
            builder.deleteCharAt(builder.length() - 1);
        }
    }
}
```

Surprisingly, it gives performance boost!

![Much better](./images/e22-02.png)