# Deque
Deque接口是Queue接口的子接口，它代表一个双端队列。LinkedList也实现了Deque接口，所以也可以被当作双端队列使用

> - void addFirst(E e):将指定元素插入此列表的开头。
- void addLast(E e): 将指定元素添加到此列表的结尾。
- E getFirst(E e):  返回此列表的第一个元素。
- E getLast(E e): 返回此列表的最后一个元素。
- boolean offerFirst(E e): 在此列表的开头插入指定的元素。
- boolean offerLast(E e):  在此列表末尾插入指定的元素。
- E peekFirst(E e):  获取但不移除此列表的第一个元素；如果此列表为空，则返回 null。
- E peekLast(E e):  获取但不移除此列表的最后一个元素；如果此列表为空，则返回 null。
- E pollFirst(E e): 获取并移除此列表的第一个元素；如果此列表为空，则返回 null。
- E pollLast(E e): 获取并移除此列表的最后一个元素；如果此列表为空，则返回 null。
- E removeFirst(E e):  移除并返回此列表的第一个元素。
- boolean removeFirstOccurrence(Objcet o): 从此列表中移除第一次出现的指定元素（从头部到尾部遍历列表时）。
- E removeLast(E e): 移除并返回此列表的最后一个元素。
- boolean removeLastOccurrence(Objcet o):     从此列表中移除最后一次出现的指定元素（从头部到尾部遍历列表时）。

ArrayDeque是实现Deque接口的类
