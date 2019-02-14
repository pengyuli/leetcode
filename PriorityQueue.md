# PriorityQueue
PriorityQueue是Queue接口的实现类

```
PriorityQueue()
PriorityQueue(int initialCapacity)
PriorityQueue(int initialCapacity, Comparator<? super E> comparator)
PriorityQueue(Comparator<? super E> comparator)

```
PriorityQueue保存元素的顺序不是按照入队顺序, 而是按照大小排序.因此当调用peek()或pool()方法取出队列中头部的元素时，并不是取出最先进入队列的元素，而是取出队列中的最小的元素。

1. 自然排序
Queue会调用集合中元素所属类的compareTo(Object obj)方法来比较元素之间的大小关系，然后将集合元素按升序排列，即把通过compareTo(Object obj)方法比较后比较大的的往后排。这种方式就是自然排序

	```
	PriorityQueue<Integer> queue = new PriorityQueue<Integer>();
	```

2. 订制排序
定制排序是通过Comparator接口的帮助。该接口包含一个int compare(T o1,T o2)方法，该方法用于比较o1,o2的大小：如果该方法返回正整数，则表明o1大于o2；如果该方法返回0，则表明o1等于o2;如果该方法返回负整数，则表明o1小于o2。

	```
	public static Comparator<Customer> idComparator = new Comparator<Customer>(){
	 
	        @Override
	        public int compare(Customer c1, Customer c2) {
	            return (int) (c1.getId() - c2.getId());
	        }
	    };
	 }
	 
	 Queue<Customer> customerPriorityQueue = new PriorityQueue<>(7, idComparator);
	 
	 Queue<Integer> pq = new PriorityQueue<Integer>(11,
                new Comparator<Integer>() {
                    public int compare(Integer i1, Integer i2) {
                        return i2 - i1;
                    }
                });
	 
	```
	简便的写法
	```
	PriorityQueue<Interval> pq = new PriorityQueue<>((a, b) -> a.start - b.start);
	```


- 添加:queue.offer()

- 移除:queue.poll()

- 查看:queue.peek()

插入方法（offer()、poll()、remove() 、add() 方法）时间复杂度为O(log(n)) ；
检索方法（peek、element 和 size）时间复杂度为常量。

###线程
队列的实现不是同步的。不是线程安全的。如果多个线程中的任意线程从结构上修改了列表， 则这些线程不应同时访问 PriorityQueue实例。保证线程安全可以使用PriorityBlockingQueue 类。


