---
layout: post
title: "Javascript Operators"
comments: true
categories: JavaScript
---

#### <u><b> Javascript Operators </b></u>

1. String concatnation
<pre>
    console.log('my' + ' cat');
    console.log('1' + 2);
    console.log(`string literals: 1+2 = ${1 + 2}`);
</pre>

2. Numeric operators
<pre>
    console.log(1 + 1 - 1 / 1 * 1 % 2 ** 2);
</pre>

3. Increment(decrement) operators
<pre>
    let counter = 1;
    const preIncrement = ++counter;
    console.log(`preIncrement: ${preIncrement}, counter: ${counter}`);
    const postIncrement = counter ++;
    console.log(`preIncrement: ${preIncrement}, counter: ${counter}`);
</pre>

4. Assignment operators
<pre>
    let x = 1;
    let y = 2;
    x += y;
</pre>

5. Comparison operators
<pre>
    console.log(1 < 3);
    console.log(1 > 3);
    console.log(1 <= 3);
    console.log(1 >= 3);
</pre>

6. Logical operators : ||, &&, !
- expression 중 연산이 많은 것을 뒤에 배치한다!! ||와 &&이 혼합되어 있다면 &&을 가장 뒤에!
<pre>
    const value1 = false;
    const value2 = 4 < 2;
    console.log(`or: ${value1 || value2 || check()}`);

    function check(){
        for (let i = 0; i < 10; i++){
            console.log('wasting time');
        }
        return true;
    }
</pre>

7. Equality
<pre>
    const stringFive = '5';
    const numberFive = 5;

    console.log(stringFive == numberFive);
    console.log(stringFive === numberFive);

    // object equality by reference
    const a = {name: "a"};
    const b = {name: "b"};
    const c = a;
    console.log(a == b); // false
    console.log(a === b); // false
    console.log(a == c); // true 
    console.log(a === c); // true

    // equality - puzzler
    console.log(0 == false); // t
    console.log(0 === false); // f
    console.log('' == false); // t
    console.log('' === false); // f
    console.log(null == undefined); // t
    console.log(null === undefined); // f
</pre>

8. Conditional operators: if
<pre>
    const name = 'a';
    if (name === 'a'){
        console.log("Welcome a");
    }else if(name === 'coder'){
        console.log('Welcome b');
    }else{
        console.log('unknown');
    }
</pre>

9. Tenary operator: ?
- condition ? value1 : value2;
- 간단할 때만 사용. 복잡할땐 if나 switch
<pre>
    console.log(name === 'a' ? 'yes' : 'no'); 
</pre>

10. Switch statement 
- use for multiple if checks(if 문에서 else if 가 많을 때 사용.)
- use for enum-like value check
- use for multiple type checks in TS
<pre>
    const browser = 'IE';
    switch (browser){
        case 'IE':
            console.log('go away!');
            break;
        case 'Chrome':
        case 'Firefox':
            console.log('good');
            break;
        default:
            console.log('same all!');
            break;
    }
</pre>

11. Loops

<pre>
    // while loop, while the condition is truthy, body code is executed
    let i = 3;
    while (i > 0){
        console.log(`while: ${i}`);
        i--;
    }

    // do while loop, body code is executed first, then check the condition.
    do {
        console.log(`do while: ${i}`);
        i--;
    } while (i > 0);
</pre>

12. For loops; consider the time complexity. Try to avoid nested loops which results in O(n^2)
<pre>
    for(let i = 0; i < 10; i++){
        for (let j = 0; j < 10; j++){
            console.log(`i: ${i}, j:${j}`);
        }
    }
</pre>