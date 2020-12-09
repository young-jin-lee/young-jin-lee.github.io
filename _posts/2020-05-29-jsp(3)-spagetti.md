---
layout: post
title: "Spaghetti"
comments: true
categories: Java
---

__Jasper의 단점과 MODEL 1의 등장 배경__

Jasper가 미리 생성해주는 내장 객체의 사용과 지시 블럭을 통해 많은 부분이 편해졌다.

~~~
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%

	/* By Jasper, the internal instance 'out' is already declared. */
	/* PrinterWriter out = response.getWriter(); */
	
	String cnt_ = request.getParameter("cnt");
	
	int cnt = 100;
	if(cnt_ != null && !cnt_.equals(""))
		cnt = Integer.parseInt(cnt_);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<% for(int i=0; i < cnt; i ++) { %>
		안녕 Servlet!!<br >
	<% } %>
</body>
</html>
~~~

하지만 Jasper를 많이 사용하는 방법이 너무 다양해서 스파게티 코드가 되는 단점이 있다. 따라서 요즘에는 Jasper를 출력에만 초점을 맞춰 사용하는 경향이 있다고 한다.
아래 코드처럼 코드가 깔끔하지 않을뿐더러 스케일이 커지면 굉장히 복잡한 스파게티 코드가 된다.
그러면 유지 보수도 어렵게 된다. 

~~~
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
   
<% 
	int num = 0;
	String num_ = request.getParameter("n");
	if(num_ != null && !num_.equals(""))
		num = Integer.parseInt(num_);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<% if(num%2 != 0){ %>
	홀수입니다.
	<% } 
	else
	{ %>
	짝수입니다.
	<% } %>
</body>
</html>
~~~

이런 단점을 보완하기 위해 MVC model 1 이 나왔다.