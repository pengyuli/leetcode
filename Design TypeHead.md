# Design TypeHead

## Clarify
### Traffic

DAU = 500m

Search = 4\*6\*500m = 12b (6times/day , type 4 letters 敲了四次字母相当于使用了typeHead 4次)

QPS = 12b/86400 = 138K

Peak = QPS * 3 = 414k




## Storage
我们可以拿到的数据:

|prefix|count|
|----|----|

### Cache

|prefix|words|
|----|----|
|a|[amazon, airbnb, apple]|
|ad|[adobe, advil]|


以为速率要求, 所以必须要databse+cache 或者都存cache

### Trie

Trie存在机器的内存中

How to store the data:

1. **TrieNode里面存top k 数据** ->内存要求很大 但是对partition比较合理
2. TrieNode里面存 top k词条的node reference 和node 频率 但是对partition可能不友好





建树: Trie自下而上的build, 每个node去call子node得到自己的top k的词条

定期把dataColeectionService里的更新的关键词建trie, 存入host



为什么trie比cache好??

## Service

### Query Service
去trie里查找, 返回最高频词
### DataCollection Service
把统计好的热门词, 生成一个trie,然后upload 到queryService

Update Trie: 

1. 假设有两个host, 先update1个, 然后take down 另外一个 继续update
2. master slave 模式update

## Scale
### Data Partition

1. 按照字母分, A机器(A\*) B机器(B\*) 但是这样会导致unbalance data

2. 按照host 大小

   Server 1, A-AABC
   Server 2, AABD-BXA
   Server 3, BXB-CDA

3. 以prefix的hash做partition 这样每次搜索会query所有的host

#### collection Service收集数据
1. 不记录所有的 而是每次用户搜索关键词 1~1000随机抽取一个数, 如果是1才存, 这样减少了1000倍的写操作.





## Reference 

https://www.educative.io/collection/page/5668639101419520/5649050225344512/5076324926357504

https://www.youtube.com/watch?v=us0qySiUsGU&t=905s (最好再看一下这个解法, 用zookeeper来分配host以及traffic)


