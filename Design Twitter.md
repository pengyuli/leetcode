# Design Twitter
## Scenario

#### UeseCase:
1. User post new tweets
2. User follow other users
4. display a news feed



storage: 100M DAU * (280 + 30) bytes => 30GB/day


## Storage
#### Follow table (SQL/NOSQL)
| Follower | Followee |
| -------- | -------- |
|          |          |


#### TweeTable 
| TweetId | UserId | Content | TweeLatitude | TweeLangitude | CreationDate |
| ------- | ------ | ------- | ------------ | ------------- | ------------ |
| PK      |        |         |              |               |              |


#### UserTable
| UserId | UserName | Email | DateOfBirth | CreationDate | LastLogin |
| ------ | -------- | ----- | ----------- | ------------ | --------- |
| PK     |          |       |             |              |           |
### Data sharding

##### Shard by **UserId**

1. 如果有Famous people会导致对应的shard流量过大
2. 有的人照片多 有的人照片少， data不均分

##### Shard by TwitterId

newsFeed生成的过程是先找到所有follow的人， 然后query所有人最近k条twee然后根据time合并。 这样sharding会导致query所有的d'b

##### Shrad by creation time

这样读取会可以只query一部分db, 但是写load都在同样的db上

##### Shard by time+twitterId

twitterId= epoch time + increment number

仍然会query所有db, 但是合并时候可以根据tweeId合并

## NewsFeed

### Pull Model

1. get all users that a followed
2. get top K tweet of these users
3. merge and get the top K

缺点: 在getTweet时候n个人就有n次db read 比较慢， 尽管在twitter中我们会cache相应的data, 所以读取一个人的twitter因该都是从cache中读取， 但是仍旧是浪费资源



### Fan out Model

##### NewsFeed

We can store this data in a cache where the “key” would be UserID and “value” would be a STRUCT like this:

```
Struct {
    LinkedHashMap<FeedItemID, FeedItem> feedItems;
    DateTime lastGenerated;
}
```



We can store FeedItemIDs in a data structure similar to [Linked HashMap](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedHashMap.html) or [TreeMap](https://docs.oracle.com/javase/6/docs/api/java/util/TreeMap.html), which can allow us to not only jump to any feed item but also iterate through the map easily. 



每次follow的人发消息, 都会异步push到你的newsFeedCache中, 每次只要去读前k条信息.

缺点: 当你的follower比较多时候, 异步会比较慢, 所以每个人得到信息世界不同步

可以明星用pull, 普通人push 最后合并

## Scale

#### Cache 

Read heavy -> Caching

Method : LRU + cache last 3 days of data



> Our cache would be like a hash table where ‘key’ would be ‘OwnerID’ and ‘value’ would be a doubly linked list containing all the tweets from that user in the past three days. Since we want to retrieve the most recent data first, we can always insert new tweets at the head of the linked list, which means all the older tweets will be near the tail of the linked list. Therefore, we can remove tweets from the tail to make space for newer tweets.



### like function

example like twee

like table

| likeId | tweeId | userId | timeStamp | status                 |
| ------ | ------ | ------ | --------- | ---------------------- |
|        |        |        |           | 用来表示like是否还有效 |

Activity table

| tweeId | likes |
| ------ | ----- |
|        |       |

用increment的方法来记录likes的多少, 因为每次query like table会导致很慢



