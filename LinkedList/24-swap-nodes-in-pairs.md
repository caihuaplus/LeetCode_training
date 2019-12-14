# 24. 两两交换链表中的节点

> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
>
> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
>
> 示例:
>
> ```
> 给定 1->2->3->4, 你应该返回 2->1->4->3.
> ```

## 解题思路

本题的递归和非递归解法其实原理类似，都是更新每两个点的链表形态完成整个链表的调整。

递归写法要观察本级递归的解决过程，形成抽象模型，**因为递归本质就是不断重复相同的事情**。而不是去思考完整的调用栈，一级又一级，无从下手。

要关心的是

1. 返回值
2. 递归的调用单元做了什么
3. 终止条件

在本题中

1. 返回值：交换完成的子链表
2. 递归的调用单元做了什么：设置要交换的两个节点 head 和 next，head.next 指向 next.next，next.next 指向 head，完成交换。
3. 终止条件：head 节点为 null，或者 head.next 节点为 null，无可交换的结点。

### 1. 递归代码

```java
public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode next = head.next;
    head.next = swapPairs(next.next);
    next.next = head;
    return head;
}
```

### 2. 非递归代码

```java
public ListNode swapPairs(ListNode head) {
    ListNode prev = new ListNode(-1);
    prev.next = head;
    ListNode temp = prev;
    while (temp.next != null && temp.next.next != null) {
        ListNode start = temp.next;
        ListNode end = temp.next.next;
        temp.next = end;
        start.next = end.next;
        end.next = start;
        temp = start;
    }
    return prev.next;
}
```