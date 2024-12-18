---
{"dg-publish":true,"permalink":"/study/dsa/data-structures/linked-list/","dgPassFrontmatter":true}
---


# Reversing a linked list
- You need to keep returning the end of the list
	- i.e., Keep returning the first "next" variable as that is the final solution.
	- You do a `head.next.next = head` because you are telling your next to refer back to yourself. this is the act of reversing.
```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode next = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return next;
    }
}
```

