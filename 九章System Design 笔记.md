
# 九章笔记

# Messenger

## Service

- Message Service

- Real time service

## Storage
因为如果把所有信息都存在message table会导致查询非常慢(比如查询两个人的聊天记录, 群里所有聊天记录), 所以对于message system把一个对话存为一个Thread.
#### Message table(NoSQL)
- UserCase: use threadId to get all message in thread and rank in time

- RowKey(Sharding Key) Thread_ID
- Column Key: time


|Id|Thread_Id|user_ID|content|Time|
|----|----|----|----|----|
|Int|int|string|text|time|
| | |谁发的|内容| |

#### Thread table (SQL)
- UseCase:
	1. 按照thread_ID 拿到所有消息(进入thread)
	2. 查询user_ID所有的thread()
- RowKey user_ID, ThreadID
- columnKey user_ID+last_update_time

|Thread ID|UserID|participant_ids|Created_time|Last_update_time|is_muted|isblock|
| ---- | ---- | ----| ---- |---|---|---|
|int|string|text|time|time|
| ---- | 谁的thread | ----| ---- |index= true|---|---|

但是我们需要支持对于一个用户, 查询他所在的所有thread(登录app界面), 所以要么把user_ID也加入thread table, 让key变成user_id+thread_ID这样一个thread存n份, 或者另起一个user-Trhead table里面记录user和thread的对应关系.

### solution
Send:

- client把消息内容和接受者发给server
- server先查看thead存在/创建thread (因为这里thred table不支持根据user来找thread, 这里可以把thread_ID 设为userA_userB, 这样就可以根据thread_ID 查找了)
- 创建一条message

Receive

- Pull 每隔n秒向服务器pull一下messgge
- Push 创建一个push Service(Real time Service 一部分), push servie 和client通过socket想联通, message service通过push service把message传递给client


#### Online Status table (Cache)
|userId |Last_time|Server connected|
|----|----|----|

维护一个heart beat service
- 用户上线以后每隔2-5秒向server heart beat一次
- 在线好友每隔3-5秒向server要一次大家的状态
- cache里还存着那个server和user相连, 这样当需要push信息to user时候知道通过哪个server和user相连


## Scale
#### Group Message
因为如果用push model,会有好多人不在线, 群体push load大, 但是效率又低. 所以可以增加一个channel service, 在线的用户就订阅到channel上面. CHannel service 数据用cache存储

用户上线时webserver找到user所有的群, 然后通知channel service订阅

如果用户断线, push service通知channel service把用户移除

MessageService --> channel service --> pushService





# Tiny URL

## Scenario

### Use case

1. give short URL return long URL
2. give long URL return short URL  

假设 100M DAU
创建:
QPS = 100M\*0.1/86400 = 100;
peak QPS = QPS \* 3 = 300;

点击:
QPS = 100M\*1/86400 = 1000;
peak QPS = QPS \* 3 = 3000;

每天产生 100M\* 0.1 ~ 10M URL
假设一条url 0.1k
0.1k\*100M = 1GB 的data

读>>写, 要用cache优化

## Service

### URLService

- URLService.encode(long_url)
- URLService.decode(short_url)

## Storage

实现生成short URL有两种方法

### 1. 随机生成+数据库去重

#### Database :

1. SQL 分别对long URL 和short URL建立index
2. NoSQL 用两张表存储

### 2. 根据递增id生成URL

因为url是a-z A-Z 0-9 62个组成可以把sequence id 转化成62进制, 然后存为shortURL

#### DataBse:

因为用sequence ID 所以必须SQL, 可以两张表分别存long->short 和short->long

也可以用递增的sequentialId->long URL table
这样只需要一个table(index: longURL)

#### ZooKepper

用zookepper把一共能生成的url个数, 划分为n个区, 然后每个host(worker thread)去拿一个区域, 然后在自己的server上counter+1生成URL. 如果用完了再去拿

## Scale

### Reduce response time

Cache 读多

# Design Web Crawler

Use case:

要确定是重复的爬还是就爬一次



## Scenario

### calculation 

- Assume 15billion pages within 4 weeks
  - 15B / (4 weeks * 7 days * 86400 sec) ~= 6200 pages/sec
- Storage
  - 15B*100kb = 1.5Petabytes



### High level design

BFS crawl

1. Pick a URL from the unvisited URL list.
2. Determine the IP Address of its host-name.
3. Establish a connection to the host to download the corresponding document.
4. Parse the document contents to look for new URLs.
5. Add the new URLs to the list of unvisited URLs.
6. Process the downloaded document, e.g., store it or index its contents, etc.
7. Go back to step 1





## Storage & Service

需要两个storage, 一个来存crawl下来的web contentd的webstorage,  一个用来作为crawler的queue and hash table(UrlStorage)



Crawler每次可以向UrlStorage拿1000个URL来craw(防止高频率的读写出现concurrency)

Crawler每次crawl下来的网站content, 存入webstorage, 然后提取出来URL放入UrlStorage. 

Task Table

| Id     | url  | state    | priority | available_time |
| ------ | ---- | -------- | -------- | -------------- |
| userId | URL  | crawling | 1        | time           |

Crawler用BFS方式读URL 写入table.

Crawler在爬的网页会把state改成working

Priority是在好多available时候优先处理的网页

Available time控制抓取频率, 



### Shard task table

- Horizontal sharding



## Scale

### Isolate pages

path-ascending crawling



### How to handle update for failure

- Exponential back-off
  - Success: crawl after 1 week
  - no.1 failure: crawl after 2 weeks
  - no.2 failure: crawl after 4 weeks
  - no.3 failure: crawl after 8 weeks

### How to handle dead cycle

- Too many web pages in sina.com, the crawler keeps crawling sina.com and don't crawl other websites
- Use quota (10%)

## Others

#### bloom fileter

check not exist 100% 

check exist 90% correctness

优点: 占用空间极小



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

| prefix | words                   |
| ------ | ----------------------- |
| a      | [amazon, airbnb, apple] |

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

