# 141. 环形链表

>给定一个链表，判断链表中是否有环。
>
>为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
>
>
>
>示例 1：
>```
>输入：head = [3,2,0,-4], pos = 1
>输出：true
>解释：链表中有一个环，其尾部连接到第二个节点。
>```
>
>示例 2：
>
>```
>输入：head = [1,2], pos = 0
>输出：true
>解释：链表中有一个环，其尾部连接到第一个节点。
>```
>
>示例 3：
>
>```
>输入：head = [1], pos = -1
>输出：false
>解释：链表中没有环。
>```
>
>进阶：
>
>你能用 O(1)（即，常量）内存解决此问题吗?

## 解题方法

### 1. 哈希表

遍历所有结点并在哈希表中存储每个结点的引用（或内存地址）。

如果当前结点为空结点 null（即已检测到链表尾部的下一个结点），那么已经遍历完整个链表，那么返回 false（该链表**不是**环形链表)。

如果当前结点的引用已经存在于哈希表中，那么返回 true（即该链表为环形链表)。

```java
public boolean hasCycle(ListNode head) {
    Set<ListNode> nodeSet = new HashSet<>();
    while (head != null) {
        if (nodeSet.contains(head)) {
            return true;
        } else {
            head = head.next;
            nodeSet.add(head);
        }
    }
    return false;
}
```

复杂度分析：

- 时间复杂度：O(n)。

- 空间复杂度：O(n)。

### 快慢指针

通过使用具有 **不同速度** 的快、慢两个指针遍历链表，空间复杂度可以被降低至 O(1)。慢指针每次移动一步，而快指针每次移动两步。

如果列表中不存在环，最终快指针将会最先到达尾部，此时返回 false。

如果是一个环形链表，快指针最终会套慢指针的圈，二者相等时，返回 true。

```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) return false;
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        // 不相等，判断快指针是否为 null，或者指向的是否为 null
        if (fast == null || fast.next == null) return false;
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

复杂度分析：

- 时间复杂度：O(n)。

- 空间复杂度：O(1)。
