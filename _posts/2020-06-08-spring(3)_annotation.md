---
layout: post
title: "Annotations"
comments: true
categories: Java
---

__DI with Spring__

Spring을 이용하여 Dependency Injection을 하는 방법은 두 가지이다.

- Xml 파일 설정
- Annotation 활용

여기서는 더 모던한 방법인 Annotation 활용 방법을 소개한다. 

@Autowired는 세 군데에 사용할 수 있다. 
- 필드 위(추천)
- 오버로드 생성자 위
- setter 위

<pre>
public class InlineExamConsole implements ExamConsole
{
    @Qualifier("exam2")
    @Autowired(required=false)  // 기본 생성자가 호출 되면서 바인딩(injection 됨)
    private Exam exam;

    @Qualifier("exam2")
    @Autowired(required=false)
    public InlineExamConsole(@Qualifier("exam2")Exam exam){
        this.exam = exam;
    }

    @Qualifier("exam2")
    @Autowired(required=false)
    public void setExam(Exam exam){
        this.exam = exam;
    }
}
</pre>

