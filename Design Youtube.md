# Design Youtube

## Clarify

### function

1. Upload video
2. view video
3. write and view comments/likes

### Calculation

800m DAU and 5videos/day

​	800M * 5 / 86400 sec => 46K videos/sec

write: read 1:200

​	46K / 200 => 230 videos/sec

every minute 500 hours worth of videos are uploaded to Youtube

​	500 hours * 60 min * 50MB => 1500 GB/min (25 GB/sec)



## Storage

Video should be store in distributed file storage system like HDFS (not S3?)



Video MetaData table

| VideoId | Title | discription | time | user | tag  |
| ------- | ----- | ----------- | ---- | ---- | ---- |
|         |       |             |      |      |      |

Comment table

| CommentId | VideoId | UserId | Comment | time |
| --------- | ------- | ------ | ------- | ---- |
|           |         |        |         |      |

Like function 可以参考design instagram的like

