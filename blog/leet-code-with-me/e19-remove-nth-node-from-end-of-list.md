
# Episode 19 - Remove Nth node from end of list

⬅️ [Back to index](README.md)

## The task

Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example: 

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

## The solution

The task assumes that the linked list should not be converted to the array and next converted back so that it is necessary to go across the list only once. The problem is that it is necessary to remove `n`-th element from the end of the list. 

Let's assume we have a pointer `current` that ordinary points to the current element of the linked list, and a counter `elements` that shows how many elements are in the list already. 

For example, for the head element of the linked list, the `elements` is equal to zero, one element added to the list, `elements` incremented and so on. As soon as `elements` is greater or equal than `n`, it is necessary to save a reference to the first element in the new list - this is an element that goes before the elemnt we need to remove. If `elements` just greater than `n`, it is also necessary to save a reference to the element that should be removed. 

As a result, there are three pointers - `current` points to the list item that corresponds to the current iteration, `beforeRemove` points to the element that goes before the `toRemove` element. Every iteration, all three pointers should move one step forward. 

When there are no more elements in the list, it is necessary to assign `beforeRemove.next` to `toRemove.next` so that the elemnt `toRemove` will be excluded from the liked list. 

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode newHead = head; 
        ListNode current = head; 

        ListNode toRemove = head; 
        ListNode beforeRemove = head; 

        int elements = 0; 
        
        while (current.next != null) {
            elements++;
            current = current.next; 

            if (elements >= n) {
                toRemove = toRemove.next; 
                if (elements > n) {
                    beforeRemove = beforeRemove.next; 
                }
            }
        }

        if (toRemove == head) {
            newHead = newHead.next;
        } else {
            beforeRemove.next = toRemove.next; 
        }

        return newHead; 
    }
}
```

Quite elegant solution that beats 100% of other Java solutions. 

![Well done!](./images/e19-01.png)