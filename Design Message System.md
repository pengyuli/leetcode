# Design Message System

## Clarify
facebook messenger一直保存聊天记录 而whatsapp wechat一旦用户接受了就删除聊天信息, design 前要问清楚是哪一种
### Function:
1. 1:1 chat
2. user status(online/ offline)


Extend functions:

1. message sent/read/delivered
2. group message



### Storage & traffic
#### Assume: 
500million user  40message per day

#### Server number : 
a modern server can handle 50K concurrent connections at any time, we would need 10K such servers.

#### Storage amount: 
for 5 year:

500m x 40 x 0.1kb = 2TB/d
5 year: 2 x 5 x365 = 3.6PB

#### bandwidth

2TB/86400 x 2(assume peak) = 50mb/s

### Trade off need to think
#### Push vs Pull
pull: read heavy to DB, most time of the read will be empty response

pull: for group chat we may need to push to user not online, will be solve if we have user online status 

#### long pull vs websocket
LP: hanging Get, after get the response will sand another response.

WS: provide persistent connection between user and server
 

## Storage

### User status & user server connection
user status记录user 在线状况

user server 记录user和哪个server相连

因为是用websocket相连, 所以user会跟server heartbeat, 这样就可以把user, server node, heartbeat time 存在一起. 也可以分开存. 

Storage 选 cache, 因为数量不多 读>> 写

### User server endpoint

在server内部, 可以有一个map, Key: userId Value: connection object 用来track of all the opened connection to redirect messages to the users efficiently

### Message

##### Conversation table
User1 User2 ConversationId

#### Message table
MessageId ConversationId FromUser ToUser time text 

#### Read table
ToUser messageId Time

#### Unread table
FromUser MessageId Time

## Serveice
### Use case
UserA connect to N1 and UserB connect to N2 and UserC is not connet to server.

#### case 1 UserA send to UserB
1. UserA send message to N1 {Message: ConverSationId, ToUser, Text}
2. N1 go to user_server_connection cache find UserB connect to N2
3. N1 send message to N2 and also save message to DB(message table) and N1 will send back a "sent" message to UserA
4. N2 use own map find the connection obj of UserB and send message through connction obj.
5. When UserB received message and will send a "read" message back

#### case 2 UserA send to UserC
1. UserA send message to N1 {Message: ConverSationId, ToUser, Text}
2. N1 save message to DB and send back a sent message.
3. N1 go to user_server_connection cache find UserC is not connected
4. N1 then save this message to unread table
5. UserC come online connect to N3 and pull the unread table and find the message and delete the message on unread table.
6. UserC send a "read" back.
7. Assume UserA is offline now and N3 will write the message to read table.
8. when UserA come onlie he will pull the read table and know the message has been read.

## Follow up
### Group chat
we can have seperate GroupChat table
GroupchatId, users

发消息to groupchat id, server可以先去遍历users, 找到所有在线的然后push message.

### Sent image
需要在message table 里加一项url 以及加一个file storage. 每次UserA send一个picture, 先存入storage 的到url 然后才存入message table/ send to receiver

Receiver接到信息自动去storage读取file


## Scale
### retention
可以将过去的conversation data按照conversationID存入S3这样的地方, 因为过去的信息读操作就会很少
### sharding
converSationId >> userId
### Fault tolerance and Replication
1. Single Server down, client side reconnect to new server, no big issue
2. need to have data replication


## Resourses
[https://www.educative.io/collection/page/5668639101419520/5649050225344512/5724160613416960](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5724160613416960)

[https://www.youtube.com/watch?v=zKPNUMkwOJE&t=635s](https://www.youtube.com/watch?v=zKPNUMkwOJE&t=635s)
