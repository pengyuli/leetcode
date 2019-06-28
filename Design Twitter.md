# Design Twitter
## Scenario

#### UeseCase:
1. User post new tweets
2. User follow other users
3. user like  a tweet
4. display a news feed
5. \* support photo and video


## Service/Storage
#### Follow table (SQL/NOSQL)
|Follower|Followee|
|---|---|
#### TweeTable 
1. NOSQL Key为TweeId 然后再来一个UserTweeTable记录每个user对应的tweeid
2. Key为UserId range Key为CreationDate, 然后second index在TweeId?

|TweetId|UserId|Content|TweeLatitude|TweeLangitude|CreationDate|
|---|---|---|---|---|---|

#### UserTable
Key UserId

|UserId|UserName|Email|DateOfBirth|CreationDate|LastLogin|
|---|---|---|---|---|---|


### Push VS PULL
#### Pull
1. get all users that a followed
2. get top K tweet of these users
3. merge and get the top K

缺点: 在getTweet时候n个人就有n次db read 比较慢

#### Push Model
##### NewsFeedTable
|ID|OwerId|Twee_Id|Create_at|
|---|---|---|---|

每次follow的人发消息, 都会异步push到newsFeedTable, 每次只要去读前k条信息.

缺点: 当你的follower比较多时候, 异步会比较慢, 所以每个人得到信息世界不同步

可以明星用pull, 普通人push 最后合并

