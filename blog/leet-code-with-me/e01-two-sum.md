# Episode 1 - Two Sum

⬅️ [Back to index](README.md)

## The task

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

## Suboptimal solution

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