---
layout: post
title: "Expression Language(EL)"
comments: true
categories: Java
---

__데이터 구조 별로 View에서 EL 이용해서 값 뽑는 방법__

1) Number

Controller
<pre>
request.setAttribute("cnt", 30)
</pre>
View
<pre>
request.getAttribute("cnt")
</pre>
View - EL
<pre>
${cnt}
</pre>

2) List

Controller
<pre>
List list = new ArrayList(){"1", "test", ...};
request.setAttribute("list", list);
</pre>
View
<pre>
((List)request.getAttribute("list").get(0))
</pre>
View - EL
<pre>
${list[0]}
</pre>

3) Map

Controller
<pre>
Map n = new HashMap("title", "제목");
request.setAttribute("n", n);
</pre>
View
<pre>
((Map)request.getAttribute("n")).get("title")
</pre>
View - EL
<pre>
${n.title}
</pre>

<hr/>

__EL 저장소__

spag.java
~~~
package com.youngenie.web;

import ...
@WebServlet("/spag")
public class Spag extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int num = 0;
		String num_ = request.getParameter("n");
		if(num_ != null && !num_.equals(""))
			num = Integer.parseInt(num_);

		String result;
		
		if(num%2 != 0)
		    result = "홀수";
		else
		    result = "짝수";
		
		request.setAttribute("result", result);
		
		String[] names = {"david", "dragon"};
		request.setAttribute("names", names);
		
		Map<String, Object> notice = new HashMap<String, Object>();
		notice.put("id", 1);
		notice.put("title", "EL is good");
		request.setAttribute("notice", notice);
		
		//forward
		RequestDispatcher dispatcher = request.getRequestDispatcher("spag.jsp");
		dispatcher.forward(request, response);
	}
}
~~~

spag.jsp
~~~
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!-- -------------------------------------------------- -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<%
	pageContext.setAttribute("result", "hello");
%>


<body>
	<%=request.getAttribute("result") %>입니다.
	${requestScope.result}<br >
	<br >
	${names[0]}
	<br >
	${notice.id}
	${notice.title}
	${result}
	${param.n}
	${header.accept}
	
</body>
</html>
~~~
