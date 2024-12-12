# Episode 1 - Two Sum

⬅️ [Back to index](README.md)

## The task

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

## Suboptimal solution

The easiest option would be to use brute force and iterate twice across all the numbers. This is the most obvious and still working solution. The problem with it is the fact that it requires O(n^2) time to work so in case of big `n` it can take enormous amount of time to find a target number. 

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        throw new RuntimeException();
    }
}
```

## Optimal solution

Another option would be to have a cache. The task states that it is necessary to know indexes of numbers but not numbers themselves so that we may store positions of visited numbers. 

For example, we've met value `3` at position `5`, so let's add it to the `visited` map. Another trick is to use this cache for lookup the second part member of our expression. 

If `target` is equal to `10`, the current number is `6` so that it is necessary to find an index of `4` (because `10 - 6 = 4`) and here our `visited` map will help us. 

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        final Map<Integer, Integer> visited = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            final int value = nums[i];
            final int remaining = target - value; 
            if (visited.containsKey(remaining)) {
                return new int[]{i, visited.get(remaining)};
            }
            visited.put(value, i);
        }

        throw new RuntimeException();
    }
}
```