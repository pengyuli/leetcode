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
