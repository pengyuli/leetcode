# Design Uber

## Scenario

1. Driver report location
2. Rider request uber, match a driver
3. Driver deny/accept a request


### QPS计算

DAU 200k driver

Driver report location every 4 seconds:

QPS = 200k /4 = 50k

Peak QPS = 150k
(150k 写操作, 需要写快的storage)

## Service & Storage
司机和用户都先和dispatch service相连, 通过dispatch service向geoService 传递位置信息.


####用GeoHash把Longitude/Latitude变成一个字符串

### GeoService 
Location Table (写多)

|DriverIdSet|GeoHash|update_time| 
|----|----|----|

DriverTable
Key driverOd
Value geoHahs, status, trip_id 




### Dispatch Service
Trip Table (读多)

|Trip_ID| driverID| riderID|Latitude|Longitude|status|Creat time|
|----|----|----|----|----|----|----|

### Use Case:

#### User:
1. 用户打车请求, triple table 创建trip, 根据geoHash在location table查询周围的司机, 得到set of drive
2. 挨个问drive是否接单
3. 如果没有就用geoHash查询更远的范围
4. 司机接单, 在trip table 中更新driver和status, 在driver table 也更新对应信息

#### Driver
1. 定期汇报自己的位置, 存到location table
2. 完成了trip后更改driver table信息使自己进入可用状态




