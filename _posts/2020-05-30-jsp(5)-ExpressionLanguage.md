---
layout: post
title: "JSP(5) - Expression Language(EL)"
comments: true
categories: Java
---

아래는 데이터 구조 별로 View에서 EL 이용해서 값 뽑는 방법이다.

#### 1. Number

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

#### 2. List
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

#### 3. Map
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

 EL 저장소

see spag.jsp, spag.java