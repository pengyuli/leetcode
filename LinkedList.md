# LinkedList

## 翻转链表
链表的基本形式是：1 -> 2 -> 3 -> null，反转需要变为 3 -> 2 -> 1 -> null。
这里需要注意的是翻转后原链表第一个元素要指向null

```java
ListNode node = head;
        ListNode pre = null;
        while (node != null) {
            ListNode tmp = node.next;
            node.next = pre;
            pre = node;
            node = tmp;
        }
        return pre;
```

### 逐步翻转链表 
相当于把then节点不停的插在pre的下一个
```
ListNode pre = cur;
        ListNode start = pre.next;
        ListNode then = start.next;
        for (int i = 0; i < n-m; i++) {
            start.next = then.next;
            then.next = pre.next;
            pre.next = then;
            then = start.next;
        }

```

## 找中点
```
while (fast.next != null && fast.next.next != null) {
    fast = fast.next.next;
    slow = slow.next;
}
slow之后的就为链表后半部分
```

## Dummy Node
Dummy node 就是在链表表头 head 前加一个节点指向 head，即 dummy -> head。Dummy node 可以保证它永远指向链表的头, 返回的时候可以直接返回dummy的下个节点. 在需要删除表头时候也要从dummy node开始进行操作.