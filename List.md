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
### 24. 两两交换链表中的节点
> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换
> 输入：head = [1,2,3,4]
> 输出：[2,1,4,3]

![](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B91.png)
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        //输入：head = [1,2,3,4]
        //输出：[2,1,4,3]
        ListNode dummy = new ListNode(0, head);
        ListNode cur = dummy;
        while (cur.next != null && cur.next.next != null) {
            //新的下一轮交换节点
            ListNode newCur = cur.next.next.next;
            //位置在第一个的临时节点
            ListNode temp = cur.next;
            //步骤一
            cur.next = cur.next.next;
            //步骤二
            cur.next.next = temp;
            //步骤三
            temp.next = newCur;
            //新节点后移
            cur = temp;
        }
        return dummy.next;
    }
}
```
### 移除倒数第n个节点
> 给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点
> 输入：head = [1,2,3,4,5], n = 2
> 输出：[1,2,3,5]
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        //倒数第n个
        //输入：head = [1,2,3,4,5], n = 2
        //输出：[1,2,3,5]
        ListNode  fast = dummy, slow = dummy;
        while (n > 0) {
            fast = fast.next;
            n--;
        }

        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        if (slow.next != null) {
            slow.next = slow.next.next;
        }

        return dummy.next;
    }
}

```
### 判断链表是否有环
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode fast = head.next, slow = head;

        while (slow != fast){
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }

        return true;

    }
}

public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head, slow = head;
        //只需要对fast指针判null就行,因为fast指针比较快
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```
### 判断链表是有有环,有的话返回环的入口
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        boolean hasCirle = false;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                hasCirle = true;
                break;
            }
        }
        if (! hasCirle) {
            return null;
        } else {
            //当我们找到相遇点后, 新使用一个指针,从头开始,和相遇指针以相同的速度移动,最终会在入口处相遇
            ListNode newPtr = head;

            while (slow != newPtr) {
                newPtr = newPtr.next;
                slow = slow.next;
            }
            return newPtr;
        }
    }
}
```
