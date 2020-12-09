---
layout: post
title: "How to write java in .jsp"
comments: true
categories: Java
---


__.jsp에서 자바 코드 작성하기__

일반적인 html 형태의 jsp 파일 안에서 변수를 선언하고 자바 코딩를 넣고 싶다면 코드 블럭을 사용하면 된다. 만약 코드 블럭을 사용하지 않으면 Jasper는 해당 내용을 화면에 그대로 출력해버린다.

코드 블럭은 <% %> 이다. 넣고 싶은 자바 코드를 블럭안에 넣으면 된다.
~~~
<% 
int x = 1;
int y = 2;
%>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
...
~~~

<hr/>

__(예시) 수식 삽입__

서블릿 코드에 y = x + 3을 자바코드로 넣고 싶다면 아래와 같이 jsp 파일에 코드블럭을 넣어주면 된다.
<pre>
<%
    y = x + 3
%>
</pre>
Jasper가 아래와 같이 서블릿 코드로 변환해준다.
<pre>
public final class calculator_jsp extends org.apache.jasper.runtime.HttpJspBase 
            implements org.apache.jasper.runtime.JspSourceDependent, org.apache.jasper.runtime.JspSourceImports
            {
                // 멤버함수, 멤버변수
                public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
                throws java.io.IOException, javax.servlet.ServletException {
                    // 지역변수 알고리즘
                    y = x + 3;
</pre>

<hr/>

__(예시) y의 값은 y__

서블릿 코드에 문자열 그대로와 변수 값을 함께 넣고 싶다면 다음과 같이 작성할 수 있을 것이다.
<pre>
y의 값은 <% out.print(y) %>
</pre>

더 간편한 방법으로는 아래와 같이 작성할 수 잇다.
<pre>
y의 값은 <%= y %>
</pre>
Jasper가 아래와 같이 서블릿 코드로 변환해준다.
<pre>
public final class calculator_jsp extends org.apache.jasper.runtime.HttpJspBase 
            implements org.apache.jasper.runtime.JspSourceDependent, org.apache.jasper.runtime.JspSourceImports
            {
                // 멤버함수, 멤버변수
                public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
                throws java.io.IOException, javax.servlet.ServletException {
                    // 지역변수 알고리즘
                    out.write("y의 값은 ");
                    out.print(y);
</pre>

<hr/>

__.jsp에 자바 메소드 작성하기__

jsp에서 위와 같이 코드블럭 안에 자바코드를 작성하면 서블릿 코드 파일에서는 '지역변수 알고리즘' 영역에 코드가 들어간다.
아래와 같이 메소드를 작성하면 service 메소드 안에 내가 작성한 메소드가 들어가는 것이므로 에러가 발생한다. 자바에서는 메소드안에 메소드를 넣을 수 없다.
<pre>
<%
    public int sum(int a, int b)
    {
        return a + b;
    }
%>
</pre>
코드블럭에 느낌표를 붙이면 '지역변수 알고리즘' 영역이 아닌 '멤버함수, 멤버변수' 영역에 코드가 삽입된다.
<pre>
<%!
    public int sum(int a, int b)
    {
        return a + b;
    }
%>
</pre>

<hr/>

__지시 코드 블럭__

초기 설정을 위한 page 지시자는 코드 블럭이라고 하지 않고, 지시 코드 블럭이라고 하기도 한다.

<pre>
<%@ page language="java" contentType="text/html; charset=UTF-8 pageEncoding="UTF-8" %>
</pre>
å
<hr/>

__코드블록 4종류 요약__

- <% %>: 일반 로직 코드 블럭   
- <%= %>: 출력 코드 블럭   
- <%! %>: 멤버변수, 멤버함수 정의 코드 블럭   
- <%@ %>: 지시 코드 블럭   

※ 내장객체: Jasper가 jsp파일을 서블릿 코드로 변환하면서 필요한 변수나 메소드를 스스로 만들어낸다. 어떤 메소드들이 있는지 알아 놓을 필요가 있다.

<center><img src="/img/java/JSP_ImplicitObjects.png" width="500" height="300"></center> 
<center><img src="/img/java/JSP_ImplicitObjects_request.png" width="500" height="300"></center> 
<center><img src="/img/java/JSP_ImplicitObjects_response.png" width="500" height="300"></center> 
<center><img src="/img/java/JSP_ImplicitObjects_out.png" width="500" height="300"></center> 
<center><img src="/img/java/JSP_ImplicitObjects_session.png" width="500" height="300"></center> 
<center><img src="/img/java/JSP_ImplicitObjects_application.png" width="500" height="300"></center> 