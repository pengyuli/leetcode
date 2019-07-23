
# 九章笔记

# Messenger

## Service

- Message Service

- Real time service

## Storage
因为如果把所有信息都存在message table会导致查询非常慢(比如查询两个人的聊天记录, 群里所有聊天记录), 所以对于message system把一个对话存为一个Thread.
#### Message table(NoSQL)
- UserCase: use threadId to get all message in thread and rank in time

- RowKey(Sharding Key) Thread_ID
- Column Key: time


|Id|Thread_Id|user_ID|content|Time|
|----|----|----|----|----|
|Int|int|string|text|time|
| | |谁发的|内容| |

#### Thread table (SQL)
- UseCase:
	1. 按照thread_ID 拿到所有消息(进入thread)
	2. 查询user_ID所有的thread()
- RowKey user_ID, ThreadID
- columnKey user_ID+last_update_time

|Thread ID|UserID|participant_ids|Created_time|Last_update_time|is_muted|isblock|
| ---- | ---- | ----| ---- |---|---|---|
|int|string|text|time|time|
| ---- | 谁的thread | ----| ---- |index= true|---|---|

但是我们需要支持对于一个用户, 查询他所在的所有thread(登录app界面), 所以要么把user_ID也加入thread table, 让key变成user_id+thread_ID这样一个thread存n份, 或者另起一个user-Trhead table里面记录user和thread的对应关系.

### solution
Send:

- client把消息内容和接受者发给server
- server先查看thead存在/创建thread (因为这里thred table不支持根据user来找thread, 这里可以把thread_ID 设为userA_userB, 这样就可以根据thread_ID 查找了)
- 创建一条message

Receive

- Pull 每隔n秒向服务器pull一下messgge
- Push 创建一个push Service(Real time Service 一部分), push servie 和client通过socket想联通, message service通过push service把message传递给client


#### Online Status table (Cache)
|userId |Last_time|Server connected|
|----|----|----|

维护一个heart beat service
- 用户上线以后每隔2-5秒向server heart beat一次
- 在线好友每隔3-5秒向server要一次大家的状态
- cache里还存着那个server和user相连, 这样当需要push信息to user时候知道通过哪个server和user相连


## Scale
#### Group Message
因为如果用push model,会有好多人不在线, 群体push load大, 但是效率又低. 所以可以增加一个channel service, 在线的用户就订阅到channel上面. CHannel service 数据用cache存储

用户上线时webserver找到user所有的群, 然后通知channel service订阅

如果用户断线, push service通知channel service把用户移除

MessageService --> channel service --> pushService
