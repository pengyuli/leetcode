# Design Web Crawler

Use case:

要确定是重复的爬还是就爬一次



## Scenario

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





## Storage & Service

需要两个storage, 一个来存crawl下来的web contentd的webstorage,  一个用来作为crawler的queue and hash table(UrlStorage)



Crawler每次可以向UrlStorage拿1000个URL来craw(防止高频率的读写出现concurrency)

Crawler每次crawl下来的网站content, 存入webstorage, 然后提取出来URL放入UrlStorage. 

Task Table

| Id     | url  | state    | priority | available_time |
| ------ | ---- | -------- | -------- | -------------- |
| userId | URL  | crawling | 1        | time           |


Crawler用BFS方式读URL 写入table.

Crawler在爬的网页会把state改成working

Priority是在好多available时候优先处理的网页

Available time控制抓取频率, 



### Shard task table

- Horizontal sharding



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