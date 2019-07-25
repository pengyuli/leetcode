# Tiny URL
## Clarify

1. what is the length of the short URL
2. is the link will be expire in certain time or expire
3. shall we return same short URL if different user put same long URL

### Function

give long url generate a short url and return

give short url redirect to long url

### Calculation
Assume 500m url/month  read:write = 1:1

read = 500m * 100 \*86400 =20kQPS

save 5 years of data. average 0.5kb/record

Storage = 500m \* 5 \* 12 * 0.5kb = 15TB

读>>写, 要用cache优化


## Service

### shortURL generation

#### Counter

可以用一个counterservice 维护一个递增的序列, 每次service call counterService to get a shortURL

但是counterService 不好scale, 因为多台机器很难share一个counter. 可以每个机器有一个counter区间, 但是因为没有master, 所以counter用尽了或者加减新的机器都会造成困难

#### ZooKepper

Zookeeper类似一个distributed coordinate service, 可以manage host 不用考虑scale之类的issue 

用zookepper把一共能生成的url个数, 划分为n个区, 然后每个host(worker thread)去拿一个区域, 然后在自己的server上counter+1生成URL. 如果用完了再去拿. 加减server host的行为都会在zookeeper注册, 并且得到分配.

缺点: if one host down, 那么他所拿到的数字段就也mark不能用了

## Storage

| shortURL | longURL |
| -------- | ------- |
| string   | string  |



## Scale
#### cache

因为读特别多, 要加cache优化

#### Partition

对shortURL做consistent hashing

#### Data Clean UP(if URL has expiration time)

- Whenever a user tries to access an expired link, we can delete the link and return an error to the user.
- A separate Cleanup service can run periodically to remove expired links from our storage and cache. This service should be very lightweight and can be scheduled to run only when the user traffic is expected to be low.





## Reference

https://www.educative.io/collection/page/5668639101419520/5649050225344512/5668600916475904

https://www.youtube.com/watch?v=fMZMm_0ZhK4

https://www.youtube.com/watch?v=JQDHz72OA3c