---
layout: post
title: "Javascript Variables"
comments: true
categories: JavaScript
---

#### <u><b> Javascript Variables </b></u>

1. Variable, rw(read/write)
- variable declaration: let (added in ES6)
<pre>
    let name = 'genie';
    console.log(name);
    name = 'hello';
    console.log(name);
</pre>

* var : DO NOT EVER USE THIS!
- 1. It causes 'HOISTING' which means 'pulling the declaration to the top.'; it results in a situation where variables can be used before their declarations and hence a spaggetti code.
- 2. ignores block scope
<pre>
    age = 4;
    var age;
</pre>

2. Block scope
<pre>
    let globalName = "globalNm";
    {
        let name = 'localNm';
        console.log(name);
        name = 'hello';
        console.log(name);
        console.log(globalName);
    }
    console.log(name); // This prints nothing for its declaration is inside the scope.
    console.log(globalName);
</pre>


3. Constant, r(read only)
- use const whenever possible.
- only use let if the variable needs to change.
<pre>
    const dayInWeek = 7;
</pre>


* NOTE!
- Immutable data types: primitive types, frozen objects (i.e. object.freeze()) ; 통째로 메모리에 올림. 그 메모리 안의 값은 못바꿈
- Mutable data types: all objects by default are mutable in JS ; 메모리안의 값을 바꿀 수 있음.
- favours immutable data types for the following reasons. eg. const
- 1. security
- 2. thread safety
- 3. reducing human mistakes

* Variable types
- variable type 1: primitive, single item (메모리에 값이 저장되어 있음.)
- ex) number, string, boolean, null, undefined, symbol
- variable type 2: container composed of multiple items, function (메모리에 레퍼런스가 있고 실제 값은 레퍼런스가 가리키는 곳에 자식 노드 개념의 메모리에 저장되어 있음.)
- object, box container
- function, first-class function