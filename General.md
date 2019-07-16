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
(for i in **map.keySet()** )

map.getOrDefault(k, 0)

map.putIfAbsent(key, value)

map.values() 转换成 arrayList ==> new ArrayList(map.values())

map.entrySet()

Map.Entry<obj, obj>

Map.Entry.getKey()

Map.Entry.getValue()
## For loop
for (char c : prefix.toCharArray())
char: string 不允许, 需要把string转化成charArray

## Character
Character.toLowerCase(char)
Character.isLetterOrDigit

## CovertToInteger问题 
注意overflow

## String
string.split("x") 把string以x切割成一个string[]
string.split("[, . ]") 把string以"," "." " "切割成一个string[]

String.valueOf(n) 把数字 char 转化成String


String.indexof('char') 

String.replaceAll("a", "b")

StringBuilder.insert(int, char)

string.equals()

string.equalsIgnoreCase()

String(charArray)

## Integer
Integer.valueOf() string 转化成int
String.valueOf() int to string

Integer.toString(int) int to string

Integer.parseInt(number)

Integer.MAX_VALUE = 2147483647 2billion...

## Comparator

```Collection.sort((a, b) -> a.start - b.start)```  以start从小到大排序

```
Collections.sort(intervals, new Comparator<Interval>() {
            public int compare(Interval i1, Interval i2) {
                return i1.start - i2.start;
            }
        });
        
Arrays.sort(intervals, (a,b) -> a.start - b.start);
```

## Arrays
把arraylist 转化成list

Arrays.asList()

Arrays.asList(1, 2)->{1,2}

arraylist.add(0, x) //addFirst

arraylist.addAll(another arraylist)//merge two arraylist

常用 (array.length + 1) /2 来判断边界

## Random
Random rand=new Random();

rand.nextInt(100); // 0-99  [0, 100)
rand.nextDouble()  // [0.0 - 1.0]
rand.nextBoolean() // true or false
