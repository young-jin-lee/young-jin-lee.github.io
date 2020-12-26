---
layout: post
title: "Database Properties"
comments: true
categories: Java
---

**Database properties 읽어오기(PropertyPlaceHolderConfigurer 이용하는 방법)**

- classpath:/config/properties 안에 local-db.properties, dev-db.properties, prd-db.properties 파일이 있다. 예시로 local-db.properties 파일의 내용은 아래와 같다.

```
db.driver=com.mysql.jdbc.Driver
db.url=jdbc:mysql://127.0.0.1:3306/test_schema?serverTimezone=Asia/Seoul
db.username=root
db.password=123456789a
```

- 먼저 local, dev, prd 라는 문자열을 변수로 담기위해 프로젝트 우측 클릭 → Run as → Run Configurations → 톰캣 클릭 → Arguments 탭 → VM arguments에 아래 내용을 등록한다.

```
-Dspring.profiles.active=local
```

- 위 설정이 되면 ${spring.profiles.active}라는 변수를 사용할 수 있게 된다.
- 톰캣에 설정하고 싶다면 톰캣안에 bin 폴더에 있는 catalina.sh에 아래와 같이 JAVA_OPTS 내용을 입력한다.

```
# Register custom URL handlers
# Do this here so custom URL handles (specifically 'war:...') can be used in the security policy
JAVA_OPTS="$JAVA_OPTS -Djava.protocol.handler.pkgs=org.apache.catalina.webresources"
JAVA_OPTS="$JAVA_OPTS -Dspring.profiles.active=prd"
```

- web.xml에서부터 연결된 context.xml 파일에는 아래와 같이 bean을 등록한다.

```
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	<property name="locations">
		<list>
			<value>classpath:/config/properties/${spring.profiles.active}-db.properties</value>
		</list>
	</property>
</bean>

<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="${db.driver}"/>
    <property name="url" value="${db.url}"/>
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean>
```

- 이외에도 같은 작업을 위해 context:property-placeholder를 이용하는 방법, <util:properties/>와 Spring EL을 이용하는 방법 등이 있다.
