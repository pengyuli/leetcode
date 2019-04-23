# BFS and DFS
DFS的边界条件判断通常在递归的开始阶段. BFS的边界判定在入队列阶段.

## BFS
bfs常被用在图或者arry中, 运用的情况为 **寻找最短路径** 和**层序遍历**

## 用Queue来实现BFS
```java
int BFS(Node root, Node target) {
    Queue<Node> queue;
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

有的时候(例如图中), 一个节点的相邻节点可能包含已经被访问过的节点, 这个时候如果再把已经访问过得节点加入Queue中就会导致程序无限循环. 所以需要用一个hashset来装已经访问过得节点

```java
int BFS(Node root, Node target) {
    Queue<Node> queue;
    Set<Node> used;
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}

```

#### Queue 的fucntion ####
Queue.offer()
q.poll()
q.peek()


## DFS ##
dfs可以用来找target是否存在问题, 

### 递归 ###
```
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}
```

### stack ###
```
boolean DFS(int root, int target) {
    Set<Node> visited;
    Stack<Node> s;
    add root to s;
    while (s is not empty) {
        Node cur = the top element in s;
        return true if cur is target;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```


