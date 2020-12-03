---
layout: post
title: "Javascript First-class Functions"
comments: true
categories: JavaScript
---

#### <u><b> Javascript First-class function </b></u>

* NOTE
- functions are treated like any other variable
- can be assigned as a value to a variable
- can be passed as an argument to other functions
- can be returned by another function

1. Function expression
- a function declaration can be called earlier than it is defined. (hoisted: very important!)
- a function expression is created when the execution reaches it.
- naming: should be a verb
<pre>
    const print = function(){ // anonymous function(function without a name)
        console.log('print');
    }
    print();
    const printAgain = print;
    printAgain();
</pre>

2. Callback functions using function expressions
<pre>
    function randomQuiz(answer, printYes, printNo){
        if (answer === 'correct'){
            printYes(); // callback function
        }else{
            printNo(); // callback function
        }
    }
    // named function <> anonymous function
    const printYes = function printY(){ // named function: used for debugging
        console.log('yes');
    }
    const printNo = function printN(){ // named function
        console.log('no');
    }
    randomQuiz('wrong', printYes, printNo);
    randomQuiz('correct', printYes, printNo);
</pre>

3. Arrow functions 
- always anonymous
<pre>
    const simplePrint = function(){
        console.log('simplePrint!');
    }
    const simplePrint = () => console.log('simplePrint!');
    const add = function(a, b){
        return a + b;
    }
    const add = (a,b) => {
        return a + b;
    }
    const add = (a,b) => a + b;
</pre>

4. IIFE: Immediately Invoked Function Expression 
- 실제 현업에서는 잘 쓰이지않는데, 바로 실행시키고 싶을때 써라.
<pre>
    (function hello(){
        console.log('IIFE');
    })();
</pre>