# Design Web Crawler
## Scenario

1. Crawl 1.6m web pages/s
	1 trillion web pages, craw once/week
2. 10 P web page storage




## Storage
Task Table

|Id|url|state|priority|available_time|
|----|----|----|----|----|

Crawler用BFS方式读URL 写入table.

Crawler在爬的网页会把state改成working

Priority是在好多available时候优先处理的网页

Available time控制抓取频率, 