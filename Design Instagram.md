# Design Instagram

## Clarify

### Function

1. upload pictures/videos
3. follow user
4. news feed

### Capacity

2M new photo/day and 200kb/picture

1day = 2M * 200KB = 400GB

10year = 1425 TB



## Service

#### Read Service和Write Service分开

> Photo uploads (or writes) can be slow as they have to go to the disk, whereas reads will be faster, especially if they are being served from cache.
>
> Uploading users can consume all the available connections, as uploading is a slow process. This means that ‘reads’ cannot be served if the system gets busy with all the write requests. We should keep in mind that web servers have a connection limit before designing our system. If we assume that a web server can have a maximum of 500 connections at any time, then it can’t have more than 500 concurrent uploads or reads. To handle this bottleneck we can split reads and writes into separate services. We will have dedicated servers for reads and different servers for writes to ensure that uploads don’t hog the system.



## Storage

Image存在file storage like S3 or HDFS



#### Follow table (SQL/NOSQL)

| Follower | Followee |
| -------- | -------- |
|          |          |


#### PhotoTable 

| PhotoId | UserId | PhoteLatitude | PhotoLangitude | CreationDate |
| ------- | ------ | ------------- | -------------- | ------------ |
| PK      |        |               |                |              |


#### UserTable

| UserId | UserName | Email | DateOfBirth | CreationDate | LastLogin |
| ------ | -------- | ----- | ----------- | ------------ | --------- |
| PK     |          |       |             |              |           |



## Scale

### Data sharding

##### Shard by **UserId**

1. 如果有Famous people会导致对应的shard流量过大
2. 有的人照片多 有的人照片少， data不均分

##### Shard by photoId

需要找到一个方法生成独特的photoId

##### Shrad by creation time

这样读取会可以只query一部分db, 但是写load都在同样的db上



### Like function

like table, 记录所有的like信息

| UserId | Status                              | time | Type         | ActivityId       |
| ------ | ----------------------------------- | ---- | ------------ | ---------------- |
|        | 这条记录是否还有效(undo like则无效) |      | Comment/post | commentId/PostId |

这个table可以找到所有like某条post的人, 但是更多时候我们需要知道like的次数, 如果用这个table

1. concurrency write
2. aggregation 很慢

所以还需要一个Activity table

| ActivityId | likeCount |
| ---------- | --------- |
|            |           |



