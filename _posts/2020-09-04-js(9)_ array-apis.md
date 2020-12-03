---
layout: post
title: "Javascript Stuff"
comments: true
categories: JavaScript
---

#### <u><b> Javascript Array APIs Problems </b></u>

1. Make a string out of an array
<pre>
        /**
        * Adds all the elements of an array separated by the specified separator string.
        * @param separator A string used to separate one element of an array from the next in the resulting String. If omitted, the array elements are separated with a comma.
        */
        join(separator?: string): string;
        
    {
        const fruits = ['apple', 'banana', 'orange'];
        const result = fruits.join(',');
        console.log(typeof(result), result);
        // output: string, apple,banana,orange
    }
</pre>
  
2. Make an array out of a string
<pre>
    /**
     * Split a string into substrings using the specified separator and return them as an array.
     * @param separator A string that identifies character or characters to use in separating the string. If omitted, a single-element array containing the entire string is returned.
     * @param limit A value used to limit the number of elements returned in the array.
     */
    // split(separator: string | RegExp, limit?: number): string[];

  {
    const fruits = '🍎, 🥝, 🍌, 🍒';
    const result = fruits.split(',',3);
    console.log(result);
    //output: ["🍎", " 🥝", " 🍌"]
  }
</pre>

3. Make [1, 2, 3, 4, 5] look like [5, 4, 3, 2, 1]
<pre>
    /**
     * Reverses the elements in an Array.
     */
    //reverse(): T[];
  {
    const array = [1, 2, 3, 4, 5];
    const result = array.reverse();
    console.log(array);
    // output: [5, 4, 3, 2, 1]
  }
</pre>
  
4. Make a new array without the first two elements.
<pre>
    /**
     * Returns a section of an array.
     * @param start The beginning of the specified portion of the array.
     * @param end The end of the specified portion of the array. This is exclusive of the element at the index 'end'.
     */
    //slice(start?: number, end?: number): T[];
  {
    const array = [1, 2, 3, 4, 5];
    const result = array.slice(2, 5);
    console.log(result);
    // output: [3, 4, 5]
  }
</pre>

* Below class 'Student' is for problem 5 ~ 10
<pre>
  class Student {
    constructor(name, age, enrolled, score) {
      this.name = name;
      this.age = age;
      this.enrolled = enrolled;
      this.score = score;
    }
  }
  const students = [
    new Student('A', 29, true, 45),
    new Student('B', 28, false, 80),
    new Student('C', 30, true, 90),
    new Student('D', 40, false, 66),
    new Student('E', 18, true, 88),
  ];
</pre>

5. Find the student with the score 90
<pre>
  /**
     * Returns the value of the first element in the array where predicate is true, and undefined
     * otherwise.
     * @param predicate find calls predicate once for each element of the array, in ascending
     * order, until it finds one where predicate returns true. If such an element is found, find
     * immediately returns that element value. Otherwise, find returns undefined.
     * @param thisArg If provided, it will be used as the this value for each invocation of
     * predicate. If it is not provided, undefined is used instead.
     */
    //find<S extends T>(predicate: (this: void, value: T, index: number, obj: T[]) => value is S, thisArg?: any): S | undefined;
    //find(predicate: (value: T, index: number, obj: T[]) => unknown, thisArg?: any): T | undefined;
  {
      const result = students.find(function(student, index){
          //console.log(student, index);
          return student.score === 90;
      });
      console.log(result);
      // output: Student {name: "C", age: 30, enrolled: true, score: 90}
  }
</pre>

* Simpler code
- if statement in the callback function is one line, you can remove 'return', ';', '{}' and add =>.
- if the fucntion is anomymous, you can remove 'function'.
<pre>
      const result = students.find((student)=>student.score === 90);
      console.log(result);
</pre>

6. Make an array of enrolled students
<pre>
    /**
     * Returns the elements of an array that meet the condition specified in a callback function.
     * @param callbackfn A function that accepts up to three arguments. The filter method calls the callbackfn function one time for each element in the array.
     * @param thisArg An object to which the this keyword can refer in the callbackfn function. If thisArg is omitted, undefined is used as the this value.
     */
    //filter<S extends T>(callbackfn: (value: T, index: number, array: T[]) => value is S, thisArg?: any): S[];
  {
      const result = students.filter((student) => student.enrolled === true);
      console.log(result);
      // output: [Student, Student, Student] ... 
  }
</pre>

7. Make an array containing only the students' scores
<pre>
  // result should be: [45, 80, 90, 66, 88]
  /**
     * Calls a defined callback function on each element of an array, and returns an array that contains the results.
     * @param callbackfn A function that accepts up to three arguments. The map method calls the callbackfn function one time for each element in the array.
     * @param thisArg An object to which the this keyword can refer in the callbackfn function. If thisArg is omitted, undefined is used as the this value.
     */
    //map<U>(callbackfn: (value: T, index: number, array: T[]) => U, thisArg?: any): U[];
  {
      const result = students.map((student)=>student.score);
      console.log(result);
      // output: [45, 80, 90, 66, 88]
  }
</pre>

8. check if there is a student with the score lower than 50
<pre>
    /**
     * Determines whether the specified callback function returns true for any element of an array.
     * @param callbackfn A function that accepts up to three arguments. The some method calls
     * the callbackfn function for each element in the array until the callbackfn returns a value
     * which is coercible to the Boolean value true, or until the end of the array.
     * @param thisArg An object to which the this keyword can refer in the callbackfn function.
     * If thisArg is omitted, undefined is used as the this value.
     */
    //some(callbackfn: (value: T, index: number, array: T[]) => unknown, thisArg?: any): boolean;
  {
      const result = students.some((student) => student.score<50);
      console.log(result);
      // output: true

      const result2 = students.every((student) => student.score<50);
      console.log(result2);
      // output:
  }
</pre>
  
9. Compute students' average score
<pre>
    /**
     * Calls the specified callback function for all the elements in an array. The return value of the callback function is the accumulated result, and is provided as an argument in the next call to the callback function.
     * @param callbackfn A function that accepts up to four arguments. The reduce method calls the callbackfn function one time for each element in the array.
     * @param initialValue If initialValue is specified, it is used as the initial value to start the accumulation. The first call to the callbackfn function provides this value as an argument instead of an array value.
     */
    //reduce(callbackfn: (previousValue: T, currentValue: T, currentIndex: number, array: T[]) => T): T;
    //reduce(callbackfn: (previousValue: T, currentValue: T, currentIndex: number, array: T[]) => T, initialValue: T): T;
  {
      const result = students.reduce((prev, curr) => {
          //console.log(prev, curr.score); 
          return prev+curr.score;
      }, 0); // 0 for the inital value
      console.log(result/students.length);
      
      const result2 = students.reduce((prev, curr) => prev+curr.score, 0);
      console.log(result2/students.length);

      // output: 73.8
  }
</pre>

10. Make a string containing all the scores that are greater than 50
<pre>
  // result should be: '45, 80, 90, 66, 88'
  {
      const result = students
        .map((student)=> student.score)
        .filter(score => score >= 50)
        .join();
      console.log(result);
  }
</pre>

- Bonus! do Q10 sorted in ascending order
<pre>
  // result should be: '45, 66, 80, 88, 90'
  /**
     * Sorts an array.
     * @param compareFn Function used to determine the order of the elements. It is expected to return
     * a negative value if first argument is less than second argument, zero if they're equal and a positive
     * value otherwise. If omitted, the elements are sorted in ascending, ASCII character order.
     * ```ts
     * [11,2,22,1].sort((a, b) => a - b)
     * ```
     */
    //sort(compareFn?: (a: T, b: T) => number): this;
  {
    const result = students
        .map((student)=>student.score)
        .sort((a,b)=>b-a)
        .join();
    console.log(result);
    // output: 90,88,80,66,45
  }
</pre>