---
layout: post
title: "Javascript Stuff"
comments: true
categories: Python
---

#### <u><b> Python Multi-processing </b></u>

1. Pool
- syntax: with Pool(number of processes) as p:

You can use a for loop to apply something to all elements in a list.
<pre>
    from multiprocessing import Pool

    def f(x):
        return x*x

    if __name__ == '__main__':
        with Pool(2) as p:
            print(p.map(f, [1, 2, 3]))
    # output: [1,4,9]
</pre>
