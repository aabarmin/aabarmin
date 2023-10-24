# Episode 2 - Add two numbers

⬅️ [Back to index](README.md)

## The task

[Challenge on LeetCode](https://leetcode.com/problems/add-two-numbers/)

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## The solution

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