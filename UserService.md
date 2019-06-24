# UserService

## Scenario
1. login/Register
2. Read user profile

## Service & Storage
### AuthService 负责登录注册
是个读多写少的操作
#### Session Table
用SQL/NoSQL + cache 优化

不能只存在cache中, 因为当一台机宕机之后, 会有很多user login,就有peak的write request.

|session_key|user_id|expire_at|
|----|----|----|
|String|String|time|

#### Use Case
##### Login
 
1. user login to service, service will create a session object (session key is uniqe) and store the data into session_table;
2. Service return the session_key as cookie
3. Browser/APP store the cookie and will use this cookie for future request to the serer.
4. server check the table to see if cookie still valid and know the user is login.

##### Logout
1. user logout and server remove the session from the table






### UserService 负责用户信息存储和查询
|UserId|userName|email|password|
|----|----|---|----|
#### use case
##### Login 
User call the service with login information and get response from service
#### update 
User call service with field to udpate



#### FriendShipService 负责好友关系
##### FriendShip table
只存一条信息的话用SQL, 可以double index, 两条信息的话NoSQL和SQL都行

|from\_user\_id|to\_user\_id|
|----|----|

#### User case
##### 单向好友关系
在database存储谁关注了谁

##### 双向好友关系
1. 存两条信息 
2. 在from和to分别加index, 然后查询时候要对两个index都做查询
