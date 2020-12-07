---
layout: post
title: "Mysql Export and Import"
comments: true
categories: database
---

### mysqldump로 데이터베이스 export/import 하기

export/import 는 툴(workbench 등)을 이용해서 할 수도 있지만, ec2/로컬의 터미널에서 작업할 수도 있다.
mysqldump는 mysql의 백업프로그램이다.

1. EXPORT

<pre>
> mysqldump -u [userName] -p -h [host] -v [dbName] [dbTable] > [exportFileName.sql]
</pre>

[userName] : root
[host] : rds endpoint
[dbName] : test_schema
[dbTable] : TEST_INFO

> : export 를 의미
> [path/exportFileName.sql] : 앞에 path/ 를 따로 입력하지 않으면 현 위치에 파일이 생긴다.

2. IMPORT

<pre>
> mysql -u [userName] -p -h [host] [dbName] [dbTable] < [exportFileName.sql]
</pre>
