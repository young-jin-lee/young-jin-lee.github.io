---
layout: post
title: "AWS Security Group"
comments: true
categories: Aws
---

### 보안 그룹 설정

- RDS에 설정된 보안그룹에 같은 VPC에 있는 EC2의 Public IP를 넣으면 EC2에서 RDS에 접근을 하지 못한다.

- VPC 내부 트래픽은 기본적으로 내부 IP로 통신한다. 내부에서 흐르는 트래픽은 VPC의 DNS resolving 옵션에 따라 Private IP로 변환된다.

ex)
ec2 -> rds hostname (my-awesome-rds.**\*\*\*\***.us-east-1.rds.amazonaws.com)

becomes

EC2: 172.31.28.146
RDS: 172.31.28.126

이렇게 얻어진 IP들은 VPC 내부 라우팅 테이블 규칙을 기반으로 통신한다.

img

라우팅 테이블은 기본적으로 트래픽과 가장 일치하는, 가장 구체적인 라우팅을 따르게 됩니다. 따라서 얻어진 private ip를 바탕으로 라우팅테이블 규칙에 따라 내부망(local)로 흘러서 다음과 같이 rds로 접속하게 되는 겁니다.

08:56:29.031621 IP ip-172-31-28-146.ec2.internal.41022 > ip-172-31-28-126.ec2.internal.mysql: Flags [S], seq 1582973972, win 62727, options [mss 8961,sackOK,TS val 3026020989 ecr 0,nop,wscale 7], length 0

08:56:29.032384 IP ip-172-31-28-126.ec2.internal.mysql > ip-172-31-28-146.ec2.internal.41022: Flags [S.], seq 3571755633, ack 1582973973, win 28960, options [mss 1460,sackOK,TS val 133711 ecr 3026020989,nop,wscale 7], length 0

https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Route_Tables.html#route-tables-priority 에서 더 자세한 내용을 확인하실 수 있습니다

두번째로, 보안그룹의 원활한 관리를 위해 ip 기반으로 허용하는것이 아닌 그룹별로 허용하는것을 추천 드립니다.

예) 두개의 보안그룹(WebServer-SecurityGroup, DB-SecurityGroup) 을 만드시고

       WebServer-SecurityGroup는 EC2에,

       DB-SecurityGroup은 mysql 데이터베이스 로 각각 속하게 해주시고

DB-SecurityGroup 의 허용목록에 WebServerSecurityGroup 에 대한 3306 (혹은 사용하고 계시는 DB포트) 허용을 해주시면 아이피로 관리하시는 것 보다 더 원활하게 사용 하실 수 있습니다.

img

세번째로, DB는 Private Subnet에, 인터넷 통신이 필요한 리소스는 Public Subnet에 위치시키는것을 추천드립니다. 이렇게 하면 더 안전하게 DB를 보호하실 수 있습니다.
