---
layout: post
title: "NGROK"
comments: true
categories: Other
---

File → New → Spring Starter Project 

Name: yourprojectname
Type: Maven
Java Version: 8
Group: com.yourgroupname
Artifact: yourprojectname
Version: 1.0 (up to you)
Description: up to you
package: com.yourgroupname.yourprojectname (com.yourgroupname.oktochange)

click next,

downdown 'web' check 'Spring Web'
...

right click the package 'com.yourgroupname.yourprojectname' → New → Class

Package: com.yourgroupname.yourprojectname.controller
Name: HomeController

Add @RestController on top of the class and 
inside the class add the following function

```
@RequestMapping("/index")
public String aaa(){
    return "Hello Spring Boot";
}
```

There are two home directories in Spring Boot.
1. src/main/resources/static
2. src/main/webapp (If you don't have the webapp folder, create it)



Now, add tomcat-embed-jasper to your pom.xml