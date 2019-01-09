# General knowledge
## Queue
Queue接口(interface)通常用linkedList类来实现
- 添加:queue.offer()

- 移除:queue.poll()

- 查看:queue.peek()

#### add()和offer()区别 ####
add()和offer()都是向队列中添加一个元素。一些队列有大小限制，因此如果想在一个满的队列中加入一个新项，调用 add() 方法就会抛出一个 unchecked 异常，而调用 offer() 方法会返回 false。因此就可以在程序中进行有效的判断！

#### poll()和remove()区别 ####
remove() 和 poll() 方法都是从队列中删除第一个元素。如果队列元素为空，调用remove() 的行为与 Collection 接口的版本相似会抛出异常，但是新的 poll() 方法在用空集合调用时只是返回 null。因此新的方法更适合容易出现异常条件的情况。

#### element() 和 peek() ####
element() 和 peek() 用于在队列的头部查询元素。与 remove() 方法类似，在队列为空时， element() 抛出一个异常，而 peek() 返回 null。

## Stack
Stack is Class
- 添加:stack.push()

- 移除:stack.pop()

- 查看:stack.peek()

## Map
(for i in map.keySet())

map.getOrDefault(k, 0)

## For loop
for (char c : prefix.toCharArray())
char: string 不允许, 需要把string转化成charArray