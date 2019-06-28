# Design TypeHead

## Scenario
DAU = 500m

Search = 4\*6\*500m = 12b (6times/day , type 4 letters 敲了四次字母相当于使用了typeHead 4次)

QPS = 12b/86400 = 138K

Peak = QPS * 3 = 414




## Storage
我们可以拿到的数据:

|prefix|count|
|----|----|


方法1:

|prefix|words|
|----|----|
|a|[amazon, airbnb, apple]|
|ad| [aidas, adobe]

以为速率要求, 所以必须要databse+cache 

方法2:
Trie放到query service里.

定期把dataColeectionService里的更新的关键词建trie, 存入host

## Service

### Query Service
去trie里查找, 返回最高频词
### DataCollection Service
把统计好的热门词, 生成一个trie,然后upload 到queryService

## Scale
#### 如果内存装不下?
1. 每个queryService分为小trie
2. 用prefix算hash, 用constant hashing 方法, 存trie

#### 提高速度
在local CDN加cache

#### collection Service收集数据
1. 不记录所有的 而是每次用户搜索关键词 1~1000随机抽取一个数, 如果是1才存, 这样减少了1000倍的写操作.


