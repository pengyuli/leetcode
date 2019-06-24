# Tiny URL
## Scenario
1. give short URL return long URL
2. give long URL return short URL  

假设 100M DAU
创建:
QPS = 100M*0.1/86400 = 100;
peak QPS = QPS * 3 = 300;

点击:
QPS = 100M*1/86400 = 1000;
peak QPS = QPS * 3 = 3000;

每天产生 100M* 0.1 ~ 10M URL
假设一条url 0.1k 
0.1k*100M = 1GB 的data


## Service 

### URLService

- URLService.encode(long_url)
- URLService.decode(short_url)

## Storage
实现生成short URL有两种方法

#### 随机生成+数据库去重

Database :
1. SQL 分别对long URL 和short URL建立index
2. NoSQL 用两张表存储

#### 根据递增id生成URL
因为url是a-z A-Z 0-9 62个组成可以把sequence id 转化成62进制, 然后存为shortURL

DataBse:
因为用sequence ID 所以必须SQL

## Scale
CDN cache