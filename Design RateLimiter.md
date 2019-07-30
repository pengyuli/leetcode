# Design RateLimiter

## Clarify
限制什么:
根据网络请求特征限制
可以是 IP, Email, User

限制程度:
n/s n/min n/h  n/d

User如果超过限制应该可以得到一个error message



## Storage & Service
RateLimiter service应该提供最小的latency, 所以应该存储在cache中

### Fix window read and write

|key|value|
|---|---|
|userId|{timestamp, count}|

1. if the ‘UserID’ is not present in the hash-table, insert it, set the ‘Count’ to 1, set ‘StartTime’ to the current time (normalized to a minute), and allow the request.

2. Otherwise, find the record of the ‘UserID’ and if CurrentTime – StartTime >= 1 min, set the ‘StartTime’ to the current time, ‘Count’ to 1, and allow the request.

3. If CurrentTime - StartTime <= 1 min and

	- If ‘Count < 3’, increment the Count and allow the request.
	- If ‘Count >= 3’, reject the request.

缺点: 
1. timestamp是fix的(11:05, 6)这样的数据
2. 读+写会禅城 race condition

解决办法: 加lock 但是会加大latency

Storage:
假设有1m user, 数据存储在redis 用redis lock

UserId 8Byte timestamp 4 byte count 2 byte + 10byte buffer

1m * (14+10) = 24mb

### sliding window set
|key|value|
|---|---|
|userId|SortedSet\<Time\>|

1. Remove all the timestamps from the Sorted Set that are older than “CurrentTime - 1 minute”.

2. Count the total number of elements in the sorted set. Reject the request if this count is greater than our throttling limit of “3”.

3. Insert the current time in the sorted set and accept the request.

Storage: UserId 8 Byre, epoch time 4 Byte, 20 byte for overhead on each set value, and we limiting 500 request/h

8 + (4 + 20 (sorted set overhead)) * 500  = 12KB

12Kb * 1m = 12 GB

### sliding window counter
if we have an hourly rate limit we can keep a count for each minute and calculate the sum of all counters in the past hour when we receive a new request to calculate the throttling limit.
|key|value|
|---|---|
|userId|{time1:count1, time2 :count2}|

We can store our counters in a Redis Hash, and set the exipre after 1 hour.

Storage:
8 + (4 + 2 + 20 (Redis hash overhead)) * 60 + 20 (hash-table overhead) = 1.6KB

1.6Kb * 1M = 1.6GB

## Scale
if a user has two request at same time and it routed to different server and both will read from cache about the count. In this case, there is race condition we will allow more than the limited count.

Solution: 
1. let load balance always point same user to same host--> load will not balance
2. put lock on cache--> latency
3. losing the reate limite



# DataDog
## scenario
1. 查询某个url访问的次数
2. 最近x年x天x小时的访问曲线图


## Storage
write >> read 因为要持久化存储, 用NoSQL

Key是url, value是所有访问记录
或者key url sortingkey time value是访问次数

写操作:
每个service汇聚15秒的数据再一起写到dataBase里, 减少写操作.  这里写不能read and write 可能造成concurrency 所以要用nosql的increment功能

retention
定期去读取分钟数据写成小时, 并删除分钟数据 以此来retention
重点:
对于数据我们分bucket存储:

- 当天数据每分钟存
- 一天到一周内的5分钟
- 一周 到一个月内的1小时
- 一月以上按照天存
