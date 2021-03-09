# Qusetion:
Given the head of a singly linked list and two integers left and right where left <= right

reverse the nodes of the list from position left to position right, and return the reversed list.
## 中文描述：
反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。
### jim result:
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    ListNode successor = null; 
    public ListNode reverseBetween(ListNode head, int left, int right) {
// base case
    if (left == 1) {
        return reverseN(head, right);
    }
    // 前进到反转的起点触发 base case
    head.next = reverseBetween(head.next, left - 1, right - 1);
    return head;
    }
ListNode reverseN(ListNode head, int n) {
    if (n == 1) { 
        // 记录第 n + 1 个节点
        successor = head.next;
        return head;
    }
    // 以 head.next 为起点，需要反转前 n - 1 个节点
    ListNode last = reverseN(head.next, n - 1);

    head.next.next = head;
    // 让反转之后的 head 节点和后面的节点连起来
    head.next = successor;
    return last;
}
}
```