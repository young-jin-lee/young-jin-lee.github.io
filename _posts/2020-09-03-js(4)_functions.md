---
layout: post
title: "Functions"
comments: true
categories: JavaScript
---

#### <u><b> Javascript Functions </b></u>

1. Function
- one function === one thing
- A function is an object in JS.
- naming: should be a VERB.
- functions always returns something. It returned undefined where return is not explicitly written in code.

2. Parameters
- primitive parameters: passed by value
- object parameters: passed by reference
<pre>
    function changeName(obj){
        obj.name = 'coder';
    }
    const a = {name:'a'};
    changeName(a);
    console.log(a);
    // output: coder
</pre>

3. Default parameters (added in ES6)
<pre>
    function showMessage(msg, frm = 'unknown'){
        console.log(`${msg} by ${frm}`);
    }
    showMessage('Hi');
    // output Hi by unknown
</pre>

4. Rest parameters (added in ES6)
- ... takes arguments as an array that contains passed parameters.
<pre>
    function printAll(...args){ 
        for (let i = 0; i < args.length; i++){
            console.log(args[i]);
        }
        // or
        for (const arg of args){
            console.log(arg);
        }
    }
    printAll('a', 'b', 'c');
</pre>

5. Early return, early exit
<pre>
    // bad to read
    function upgradeUser(user){
        if (user.point > 10){
            // long long logic
        }
    }

    // good to read
    function upgradeUser(user){
        if(user.point <= 10){
            return;
        }
        // long long logic
    }
</pre>