---
layout: post
title: "classpath"
comments: true
categories: Java
---

**classpath: 경로위치**

- web.xml에 classpath\*: 는 해당 프로젝트의 Build Path → Source에 있는 경로들을 기준으로 탐색한다.
- prj/src/main/java , prj/src/main/resources
- 따라서 아래 context-_.xml의 전체 경로는 prj/src/main/resources/spring/context-_.xml 이다.
- '\*' 표시는 어떤 단어든지 받아들이겠다는 ALL 의 의미를 가진다.

```
<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath*:spring/context-*.xml</param-value>
</context-param>
```
