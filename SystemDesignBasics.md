# System Design Basics
## NOSQL VS SQL: 

###Advantage###

1. all data in one block, easy to insert and get
2. Schema easy to change(easy to extend)
3. Built for scale (easy to scale)
4. Build for aggregation, not for 计算count, average etc.

###Disadvantage###

1. Not built for updates(consistence 不好) finincail system 不应该用nosql(not good consistence)
2. Not read optimized(无法select 一个row的data)
3. No relations
4. Join hard
5. SQL 提供自动增长的sequent ID

SQL

- 1k QPS


NoSQL

- MongoDB/Cassandra 10kQPS
- Redis/Memched 100k-1M QPS

## Single Point failur
Service SPF--> Load Balancer

Database --> data replication



# 套路

加速--> caching



spf: host 分datacenter, data replica



call 失败怎么办: retry, exponential backoff retry



host 不稳定, 随时宕机  Message queue



Data shard: consistent sharding



去重: 1. checksum 2. bloom filter