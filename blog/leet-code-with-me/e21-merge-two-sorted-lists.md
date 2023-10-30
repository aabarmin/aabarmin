# Episode 21 - Merge Two Sorted Lists

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/description/)

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

## Optimal solution

The idea is relatively simple - need to have a pointer to the `current` element in the list and another pointer to the `new head` of the list. Next, it is necessary to select an element with the smallest value from both the lists, add it to the new list and move `current` one step forward. 

```java
class Solution {
    public ListNode mergeTwoLists(ListNode first, ListNode second) {
        ListNode newHead = null; 
        ListNode current = null; 
        while (first != null || second != null) {
            final int next; 
            if (first != null && second != null) {
                if (first.val < second.val) {
                    next = first.val; 
                    first = first.next; 
                } else {
                    next = second.val; 
                    second = second.next; 
                }
            } else if (first != null) {
                next = first.val; 
                first = first.next; 
            } else {
                next = second.val; 
                second = second.next; 
            }

            if (current == null) {
                newHead = new ListNode(next);
                current = newHead; 
            } else {
                current.next = new ListNode(next);
                current = current.next; 
            }
        }
        return newHead; 
    }
}
```

Easy and intuitive solution beats 100% of other submissions. 

![Winner](./images/e21-01.png)