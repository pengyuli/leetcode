# Design RateLimiter

## Scenario
限制什么:
根据网络请求特征限制
可以是 IP, Email, User

限制程度:
n/s n/min n/h  n/d


## Storage
要记录某个特种 某个时刻 做了什么事情

可以用Cache存储数据, 因为对于rete limiter, 数据过了当前时段就没有用了.

##### Key
event + feature + timeStamp 作为memchashed key

用memchashed的increament功能

#### use case
假设为5次/min, Key 为event + feature + timeStamp

写: 每次用memeched.increament(key, ttl) 将对应的bucket访问次数+1

读:每次用event+feature+timeStamp(过去60秒的60个data)取出来, 然后计算访问次数

如果是天, 读24小时, 小时读60分钟这样分级存储


# DataDog
## scenario
1. 查询某个url访问的次数
2. 最近x年x天x小时的访问曲线图


## Storage
因为要持久化存储, 用NoSQL

Key是url, value是所有访问记录
或者key url sortingkey time value是访问次数

重点:
对于数据我们分bucket存储:

- 当天数据每分钟存
- 一天到一周内的5分钟
- 一周 到一个月内的1小时
- 一月以上按照天存


写操作:
每个service汇聚15秒的数据再一起写到dataBase里, 减少写操作

定期对database retention, 给数据瘦身
