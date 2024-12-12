# Episode 18 - 4Sum

⬅️ [Back to index](README.md)

## The task

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

```
0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
```

You may return the answer in any order.

## Suboptimal solution

One more iteration of the [3Sum](./e15-3sum.md) problem and the brute-force approach is the same: 

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        if (nums.length < 4) {
            return Collections.emptyList();
        }
        
        Arrays.sort(nums);
        final Set<List<Integer>> result = new HashSet<>();
        for (int i = 0; i < nums.length - 3; i++) {
            for (int j = i + 1; j < nums.length - 2; j++) {
                for (int k = j + 1; k < nums.length - 1; k++) {
                    for (int l = k + 1; l < nums.length; l++) {
                        int sum = nums[i] + nums[j] + nums[k] + nums[l];
                        if (sum == target) {
                            List row = Arrays.asList(
                                nums[i],
                                nums[j],
                                nums[k],
                                nums[l]
                            );
                            // Collections.sort(row);
                            result.add(row);
                        }
                    }
                }
            }
        }
        return new ArrayList(result);
    }
}
```

## Better solution

Unsurprisingly, the brute-force approach does not work well, need to do some thinking to make it better. The idea is to sequentially reduce task from `findX` to `find(X - 1)`. 

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums); 
        final Set<List<Integer>> result = new HashSet<>(); 
        fourSum(nums, 0, nums.length - 1, target, 4, new ArrayList<Integer>(), result);
        return new ArrayList(result); 
    }

    private void fourSum(
        int[] nums, 
        int left,  
        int right, 
        long target, // just because there are test scenarios with overflows
        int number, 
        List<Integer> current, 
        Set<List<Integer>> results) {

        if (number > 2) {
            // will reduce recursively up to 2
            for (int i = left; i < right + 1; i++) {
                final List<Integer> next = new ArrayList(current);
                next.add(nums[i]);
                fourSum(nums, i + 1, right, target - nums[i], number - 1, next, results);
            }
        } else {
            // need to solve 2sum problem
            while (left < right) {
                final int sum = nums[left] + nums[right];
                if (sum == target) {
                    // ok, found
                    current.add(nums[left]);
                    current.add(nums[right]);
                    results.add(new ArrayList(current));
                    // remove elements to try to find another match
                    current.remove(2);
                    current.remove(2);
                    left++;
                    right--;
                } else if (sum > target) {
                    right--;
                } else {
                    left++;
                }
            }
        }
    }
}
```

![Not bad](./images/e18-01.png)

Let's try to improve the solution: 

```patch
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums); 
        final Set<List<Integer>> result = new HashSet<>(); 
        fourSum(nums, 0, nums.length - 1, target, 4, new ArrayList<Integer>(), result);
        return new ArrayList(result); 
    }

    private void fourSum(
        int[] nums, 
        int left,  
        int right, 
        long target, // just because there are test scenarios with overflows
        int number, 
        List<Integer> current, 
        Set<List<Integer>> results) {

        if (number > 2) {
            // will reduce recursively up to 2
            for (int i = left; i < right + 1; i++) {
                final List<Integer> next = new ArrayList(current);
                next.add(nums[i]);
                fourSum(nums, i + 1, right, target - nums[i], number - 1, next, results);
            }
        } else {
            // need to solve 2sum problem
            while (left < right) {
                final int sum = nums[left] + nums[right];
                if (sum == target) {
                    // ok, found
                    current.add(nums[left]);
                    current.add(nums[right]);
                    results.add(new ArrayList(current));
                    // remove elements to try to find another match
                    current.remove(2);
                    current.remove(2);
                    left++;
                    right--;

+                   while (left < right && nums[left] == nums[left - 1]) {
+                       left++;
+                   }
+                   while (left < right && nums[right] == nums[right + 1]) {
+                       right--;
+                   }
                } else if (sum > target) {
                    right--;
                } else {
                    left++;
                }
            }
        }
    }
}
```

This simple improvement allows us to beat almost 10 more percent of submissions. 

![Better](./images/e18-02.png)

You may notice that `ArrayList` with candidates is created twice, let's remove one: 

```patch
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums); 
        final Set<List<Integer>> result = new HashSet<>(); 
        fourSum(nums, 0, nums.length - 1, target, 4, new ArrayList<Integer>(), result);
        return new ArrayList(result); 
    }

    private void fourSum(
        int[] nums, 
        int left,  
        int right, 
        long target, // just because there are test scenarios with overflows
        int number, 
        List<Integer> current, 
        Set<List<Integer>> results) {

        if (number > 2) {
            // will reduce recursively up to 2
            for (int i = left; i < right + 1; i++) {
-               final List<Integer> next = new ArrayList(current);
-               next.add(nums[i]);                
-               fourSum(nums, i + 1, right, target - nums[i], number - 1, next, results);
+               current.add(nums[i]);
+               fourSum(nums, i + 1, right, target - nums[i], number - 1, current, results);
+               current.remove(current.size() - 1);
            }
        } else {
            // need to solve 2sum problem
            while (left < right) {
                final int sum = nums[left] + nums[right];
                if (sum == target) {
                    // ok, found
                    current.add(nums[left]);
                    current.add(nums[right]);
                    results.add(new ArrayList(current));
                    // remove elements to try to find another match
                    current.remove(2);
                    current.remove(2);
                    left++;
                    right--;

                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right + 1]) {
                        right--;
                    }
                } else if (sum > target) {
                    right--;
                } else {
                    left++;
                }
            }
        }
    }
}
```

![Still can be better](./images/e18-03.png)

Adding one more skip: 

```patch
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums); 
        final Set<List<Integer>> result = new HashSet<>(); 
        fourSum(nums, 0, nums.length - 1, target, 4, new ArrayList<Integer>(), result);
        return new ArrayList(result); 
    }

    private void fourSum(
        int[] nums, 
        int left,  
        int right, 
        long target, // just because there are test scenarios with overflows
        int number, 
        List<Integer> current, 
        Set<List<Integer>> results) {

        if (number > 2) {
            // will reduce recursively up to 2
            for (int i = left; i < right + 1; i++) {
+               if (i > left && nums[i] == nums[i - 1]) {
+                   continue; 
+               }
                current.add(nums[i]);
                fourSum(nums, i + 1, right, target - nums[i], number - 1, current, results);
                current.remove(current.size() - 1);
            }
        } else {
            // need to solve 2sum problem
            while (left < right) {
                final int sum = nums[left] + nums[right];
                if (sum == target) {
                    // ok, found
                    current.add(nums[left]);
                    current.add(nums[right]);
                    results.add(new ArrayList(current));
                    // remove elements to try to find another match
                    current.remove(2);
                    current.remove(2);
                    left++;
                    right--;

                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right + 1]) {
                        right--;
                    }
                } else if (sum > target) {
                    right--;
                } else {
                    left++;
                }
            }
        }
    }
}
```

![Acceptable](./images/e18-04.png)