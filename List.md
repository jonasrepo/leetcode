# 链表
- 单链表
- 双链表
- 循环链表
```java
class ListNode{
    int val;
    private ListNode next;
    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
//leetcode submit region begin(Prohibit modification and deletion)
class MyLinkedList {
    private int  size;
    private ListNode head;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        this.size = 0;
        this.head = new ListNode(0, null);//虚拟头结点
    }

    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode cur = head;
        //得走index步, 因为有一个哑元节点
        for (int i=0; i <= index;i++) {
            cur = cur.next;
        }
        return cur.val;
    }

    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        size++;
        if (index < 0) {
            index = 0;
        }

        ListNode prev = head;//dummy -> 0 -> 1 -> 2
        for (int i=0; i< index;i++) {
            prev = prev.next;
        }
        prev.next = new ListNode(val, prev.next);
    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        ListNode cur = head;
        size--;//d -> 0 -> 1
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }

        cur.next = cur.next.next;
    }
}
```
## 常用解题思路
- 添加哑元节点
### 203 移除链表的元素
> 给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
> 输入：head = [1,2,6,3,4,5,6], val = 6
> 输出：[1,2,3,4,5]
```java
public class ListNode {
      int val;
      ListNode next;
      ListNode() {}
      ListNode(int val) { this.val = val; }
      ListNode(int val, ListNode next) { this.val = val; this.next = next; }
  }

class Solution {
    public ListNode removeElements(ListNode head, int val) {
        //[1,2,6,3,4,5,6], val = 1

        ListNode dummy = new ListNode(0, head);
        ListNode cur = dummy;
        while (cur.next != null) {
            if (cur.next.val == val) {
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        return dummy.next;
    }
}
```
### 反转链表
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        //两个指针
        ListNode cur = head;
        ListNode prev = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```