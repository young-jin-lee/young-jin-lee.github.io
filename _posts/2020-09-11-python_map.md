---
layout: post
title: "Javascript Stuff"
comments: true
categories: Python
---

#### <u><b> Python stuff </b></u>

1. map
- syntax: map(function, iterable)

You can use a for loop to apply something to all elements in a list.
<pre>
    a = [1.2, 2.5, 3.7, 4.6]
    for i in range(len(a)):
        a[i] = int(a[i])
    print(a)
    // output: [1,2,3,4]
</pre>

You can do exactly the same by using map. This is shorter and faster. 
<pre>
    a = [1.2, 2.5, 3.7, 4.6]
    a = list(map(int, a))
    print(a)
    # output: [1,2,3,4]
</pre>

2. *args
- args stands for arguments
- takes multiple arguments as elements of a tuple.
<pre>
    def myFunc(*args):
        print(args)

    myFunc(10, 20, 'a')
    # output: (10, 20, 'a')
</pre>

- If you want to pass a list and recieve it as a tuple, add an * in front of the list variable.
<pre>
    def myFunc(*args):
        print(args)

    lst = [10, 20, 'a']
    myFunc(*lst)
</pre>

3. **kwargs
- kwargs stands for keyword arguments
- takes 'x=y' form of arguments as a dictionary.
<pre>
    def myFunc(*args, **kwargs):
        print(args)
        print(kwargs)
    myFunc(10, 20,'a', x=100, y=200, z='b')
    # output: (10, 20, 'a')
    #         {'x': 100, 'y': 200, 'z': 'b'}
</pre>

<pre>
    def myFunc(*args, **kwargs):
        print(args)
        print(kwargs)
    p1 = [10, 20, 'a']
    myFunc(*p1,x=100, y=200, z='b')

    # output: (10, 20, 'a')
    #         {'x': 100, 'y': 200, 'z': 'b'}
</pre>
