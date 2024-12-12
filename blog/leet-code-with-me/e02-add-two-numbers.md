# Episode 2 - Add two numbers

⬅️ [Back to index](README.md)

## The task

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## The solution

We have two linked lists and every node in every list has a value `val`. Because it is a digit, it is `0 < val < 9` but the sum of two `val`'s might be more than 10 so that when creating a node in the result list it is necessary to keep it mind. 

The approach is simple, let's assume that the `rest` is `0` by default and while any of lists has next node, we read nodes, add `val` from one node to the `val` from another, don't forget to add `rest`. 

```java
class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }    
}

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null;
        ListNode current = null; 
        int rest = 0; 

        while (l1 != null || l2 != null || rest > 0) {
            int sum = 0; 
            if (l1 != null) {
                sum += l1.val; 
                l1 = l1.next; 
            }
            if (l2 != null) {
                sum += l2.val; 
                l2 = l2.next; 
            }
            sum += rest; 

            rest = sum / 10; 
            sum = sum % 10; 

            if (head == null) {
                head = new ListNode(sum);
                current = head;
            } else {
                final ListNode next = new ListNode(sum);
                current.next = next; 
                current = next; 
            }
        }

        return head; 
    }
}
```