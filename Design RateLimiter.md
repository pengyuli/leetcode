# Design RateLimiter
RateLimiter 的应用情况:
1. DOS attack
2. Password attamp
3. Cried card failure

## Scenario
限制什么:
根据网络请求特征限制
可以是 IP, Email, User

限制程度:
n/s n/min n/h  n/d

User如果超过限制应该可以得到一个error message

### Throttling
Throttling is the process of controlling the usage of the APIs by customers during a given period. Throttling can be defined at the application level and/or API level.


## Storage & Service

## 方法1(九章解法)
要记录某个特种 某个时刻 做了什么事情

可以用Cache存储数据, 因为对于rete limiter, 数据过了当前时段就没有用了.

##### Key
userID + feature + timeStamp 作为memchashed key

用memchashed的increament功能, timeStamp为当前分钟

#### use case
假设为50次/min, Key 为event + feature + timeStamp(当前分钟)

写: 每次用memeched.increament(key, ttl) 将对应的bucket访问次数+1

读:每次用event+feature+timeStamp(过去60分钟的60个data)取出来, 然后计算访问次数

如果是天, 读24小时, 小时读60分钟这样分级存储

#### cache 容量
UserId 8byte
feature 8byte
Count 2 byte
Time 8 byte

假设 1M用户, 限制100/h

存储数据 (8+8+2+8) x 60 x 1m = 1.6GB

### Data Sharding
用consistent hashing来做data replica and sharding. Sharding key是 memcached的key





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
