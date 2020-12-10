---
layout: post
title: "Maven - Project Settings"
comments: true
categories: Java
---

**Spring Tool Suite에서 Spring MVC 메이븐 프로젝트 세팅하기 1/3**

1. Perspective 를 Java에서 Java EE로 변경

2. 메뉴 바에서 Maven Project 선택

3. New Maven Project Popup

- 새로 세팅한다면 Create a simple project(skip archetype selection), Use default Workspace location 선택

4. New Maven Project Popup

- Artifact 세팅에서 Group Id 는 com.yourOrganizationName
- Artifact Id는 프로젝트명
- Packaging은 war 선택

5. src ➔ main ➔ webapp 안에 WEB-INF ➔ web.xml 생성

- web.xml이 없으면, pom.xml에 에러메세지가 뜬다.
- 별도로 설치되어있는 톰캣에 들어가서 복사해온다.

6. Java Resources ➔ Libraries 에서 JRE System Library 버전을 확인한다.

- 만약 변경이 필요하다면, pom.xml에서 <properties> 태그 안에 아래 내용을 입력한다.

```
<properties>
    <maven.compiler.source> 1.8 </maven.compiler.source>
    <maven.compiler.target> 1.8 </maven.compiler.target>
</properties>
```

- 프로젝트 폴더 오른쪽 클릭 후 Maven ➔ Update Project를 클릭하여 변경사항을 업데이트한다.

7. 상단 메뉴 바에 Window ➔ Preferences ➔ Web ➔ JSP Files 에서 Encoding을 UTF-8로 변경한다.

- HTML, CSS, JS Files 등에 대해서도 동일한 작업을 한다.

8. 프로젝트 오른쪽 클릭 ➔ Properties ➔ Resource에서 Text file encoding란에서 Other 선택 후 UTF-8로 바꾼다.

9. src ➔ main ➔ webapp에 index.jsp 파일 추가하기

- meat charset이 UTF-8이어야 정상이다.

10. 톰캣 X JSP 라이브러리 추가하기

- 구글에 maven tomcat api 검색
- maven repository에서 현재 적용된 톰캣 버전을 찾아 디펜던시 코드를 복사하여 pom.xml 에 붙여넣는다.

```
<dependencies>
    <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-api</artifactId>
        <version>9.0.38</version>
    </dependency>
</dependencies>
```

11. Servers 에서 버전이 맞는 apache 톰캣 서버 클릭 후 RUN하면 index.jsp를 확인해볼 수 있다.

<hr/>

**Spring Tool Suite에서 Spring MVC 메이븐 프로젝트 세팅하기 2/3**

- 이제 생성한 Maven 프로젝트에 Spring MVC를 추가한다.

12. Window ➔ show view ➔ other ➔ Maven ➔ Maven Repository 클릭

- Maven Repository 안에 보면 Global Repository, Local Repository가 있다.
- Global Repository는 메이븐 사이트(원격저장소)를 가리킨다. 클릭해보면 하위에 central이라는 이름으로 사이트 주소가 보인다.
- pom.xml 안에 dependency로 추가한 api는 Local Repository(.../.m2/repository)에 저장이 된다.
- Global Repository의 Central을 오른쪽 클릭하면 Rebuild Index, Update Index가 있는데, 이는 원격에 있는 api 목록들을 로컬로 가져오는 것이다. 다운받는 것이 아니라, 목록을 가지고 오는 것이다. 이렇게하면 필요한 api가 있을 때 마다 사이트에 들어가서 api의 <dependency> 코드를 복사 붙여넣기 하지 않고 IDE안에서 쉽게 추가 할 수 있다.(약 1~2시간 소요) 추가는 pom.xml의 dependencies 탭에서 Add를 클릭하여 추가하면 된다.
- 일단 수동으로 진행한다.

13. Maven Repository에서 Spring Web MVC api를 pom.xml에 추가한다.

```
<dependencies>
    ...
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc </artifactId>
        <version>9.0.38</version>
    </dependency>
</dependencies>
```

14. Project Explorer에서 Java Resources ➔ Libraries ➔ Maven Dependencies 에서 추가된 api를 확인한다.

- pom.xml에 추가한 spring-webmvc 뿐 아니라 여러 다른 jar 파일도 확인이 된다. 이는 maven을 사용하는 편리하고 중요한 이유 중 하나로, 어떤 api를 받아올 때 의존하는 하위 api도 함께 가지고 온다. 특정 api의 하위 api들은 pom.xml에서 Depencency Hierarchy 탭에서 확인할 수 있다.
- 일단 Dispatcher Servlet을 사용할 것이기 때문에, 아래 경로에서 DispatcherServlet.class 오른쪽 클릭하여 Copy Qualified Name을 클릭하여 이것이 가진 클래스명과 패키지명을 복사한다. 이 클래스는 프론트 컨트롤러 역할을 위한 서블릿 클래스이다.
  Java Resources ➔ Libraries ➔ Maven Dependencies ➔ spring-webmvc-0.0.0.RELESASE.jar ➔ org.springframework.web.servlet ➔ DispatcherServlet.class

15. Dispatcher Servlet이 모든 URL 요청을 받게하고, Controller는 POJO로 작성하기.

- Dispatcher Servlet을 사용하지 않고, src/main/java에 각 URL을 Controller에 매핑할 수도 있다. 하지만 그렇게 되면 dispatcher 서블릿을 컨트롤러 수만큼 만들게 되는 것이므로, 이들을 분리해서 하나의 Dispatcher Servlet이 모든 URL 요청을 받게하고, Controller는 POJO(일반 자바 클래스)로 작성하는 것이다.

- web.xml에 14번에서 복사한 Qualified Name(=org.springframework.web.servlet.DispatcherServlet.class)에서 확장자를 지우고 아래 <servlet-class> 태그 안에 넣는다. servlet-name은 마음대로 설정하고, url-pattern은 /\*를 입력한다. 모든 URL을 appServlet에서 받겠다는 의미이다.

```
<servlet>
		<servlet-name>dispatcher1</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
		<servlet-name>dispatcher1</servlet-name>
		<url-pattern>/*</url-pattern>
</servlet-mapping>
```

- 이 상태에서 서버를 띄워보면 dispatcher1-servlet.xml not found 에러가 발생한다. URL을 직접 controller에 매핑할 때는 어노테이션을 쓰면 되지만, 해당 api(dispatcher servlet)은 외부에서 작성된 파일이기에 xml에 설정을 해야하는 것이다.

16. dispatcher-servlet.xml 작성하기
