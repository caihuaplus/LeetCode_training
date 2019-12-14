# 206. 反转链表

>反转一个单链表。
>
>示例:
>```
>输入: 1->2->3->4->5->NULL
>输出: 5->4->3->2->1->NULL
>```
>进阶:
>你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

## 思路

### 1.迭代

要将链表 `1->2->3->4->5->NULL` 改为 `5->4->3->2->1->NULL`。

1. 在遍历列表时，将当前节点的 next 指针改为指向前一个元素。
2. 由于节点没有引用其上一个节点，因此必须事先存储其前一个元素。
3. 在更改引用之前，还需要另一个指针来存储下一个节点。
4. 不要忘记在最后返回新的头引用。

```java
public static ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
      ListNode nextTemp = curr.next;
      curr.next = prev;
      prev = curr;
      curr = nextTemp;
    }
    return prev;
}
```

### 2.递归

假设列表为：`n(1) ->...-> n(k-1) -> n(k) -> n(k+1) ->...-> n(n)->null`

若从节点 `n(k+1)` 到 `n(n)` 已经被反转，而我们正处于 `n(k)`。

`n(1) ->...-> n(k-1) -> n(k) -> n(k+1) <-...<- n(n)`
​
我们希望 `n(k+1)` 的下一个节点指向 `n(k)`。

所以，`n(k).next.next = n(k)`。

要小心的是 n(1) 的下一个必须指向 null 。如果忽略了这一点，链表中可能会产生循环。如果使用大小为 2 的链表测试代码，则可能会捕获此错误。

```java
public static ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```
