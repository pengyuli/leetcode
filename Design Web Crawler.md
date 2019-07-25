# Design Web Crawler



## Clarify

### Requirement

1. Is it only HTML or include(mp3, image, mp4...)

2. How many pages and are we crawl once on constant crawl



Extra requirement

网站应该有Priority  对要爬的website要有politeness

### calculation 

- Assume 15billion pages within 4 weeks
  - 15B / (4 weeks * 7 days * 86400 sec) ~= 6200 pages/sec
- Storage
  - 15B*100kb = 1.5Petabytes



### High level design

BFS crawl

1. Pick a URL from the unvisited URL list.
2. Determine the IP Address of its host-name.
3. Establish a connection to the host to download the corresponding document.
4. Parse the document contents to look for new URLs.
5. Add the new URLs to the list of unvisited URLs.
6. Process the downloaded document, e.g., store it or index its contents, etc.
7. Go back to step 1



## Service

1. DNS resolver    记录网站的host ip
2. HTML fetcher   下载document存入file storage
3. URL extracter   在document中提取出url
4. URL filter     过滤掉不要的URL(比如mp3, jpg类)
5. URL frontiner         handle priority 和politeness
6. Document 和 URL dedup check





## Storage & Service

需要两个storage, 一个来存crawl下来的web contentd 可以用S3 或者HDFS

另外一个是URL table 可以存一些url 相关的信息



如果要用checkSum来查重, 需要document和url的checksum table



### Shard task table

- sharding key : URL



## Scale

### Isolate pages

path-ascending crawling



### How to handle update for failure

- Exponential back-off
  - Success: crawl after 1 week
  - no.1 failure: crawl after 2 weeks
  - no.2 failure: crawl after 4 weeks
  - no.3 failure: crawl after 8 weeks

### How to handle dead cycle

- Too many web pages in sina.com, the crawler keeps crawling sina.com and don't crawl other websites
- Use quota (10%)


## Others
#### bloom fileter
check not exist 100% 





## Reference

https://www.youtube.com/watch?v=BKZxZwUgL3Y&t=138s

https://www.educative.io/collection/page/5668639101419520/5649050225344512/5718998062727168



check exist 90% correctness

优点: 占用空间极小