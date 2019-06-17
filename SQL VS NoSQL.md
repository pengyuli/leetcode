# SQL VS NoSQL

## NOSQL: 

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



SQL

- 1k QPS


NoSQL

- MongoDB/Cassandra 10kQPS
- Redis/Memched 100k-1M QPS
