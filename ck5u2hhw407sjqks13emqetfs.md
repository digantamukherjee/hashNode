## 7. The Strict Mode, Natives and Primitives

Moving on from the  [last post](https://diganta.hashnode.dev/6-prototype-chain-objectcreate-and-more-ck5r5bqme06w4qks1xad55e27)  let us go back to the ECMAScript(JavaScript) specifications and explore the *Strict Mode*

# The Strict Mode
JavaScript is in general a very forgiving language. It ignores a lot of stuff which other programming languages would generally start complaining about. By doing so there might be potential security issues and we live in an age where unethical hacking is quite common and prevalent, ECMAScript gives us an option to make things safer in terms of code implementation.

This variant of ECMAScript is commonly referred to as *strict mode*.

What does the strict mode offer?

- Stricter error checking by eliminating silent errors by changing them to throw errors

- Higher security by excluding some specific syntactic and semantic feature of the regular ECMAScript

- Better code optimizations can be performed

So in simple terms ECMAScript implementations like JavaScript needs to include the ability to interpret code when its written in both regular and strict(more restricted) modes. 

The strict mode is applied at a level of *syntactic code unit*(a block or a function) and so it has only local effects within such code units. More than one *strict mode* declarations may result in a warning. When processed under the *strict mode* the ECMAScript code can be categorized into **three **different types:

1. Strict Global Code
2. Strict Eval Code
3. Strict Function Code

Let's define these:

## Strict Global Code
A global code in JavaScript is strict global code if it begins with a Directive Prologue that contains the `"use strict";` Directive
### Directive Prologue : 
Its an expression statement that occurs at the initial statement of a Program or FunctionBody is made up of a *string literal* followed by a semicolon(optional as JavaScript will add one automatically if missed out)


```
"use strict";
//or 'use strict';
var a = 2;
``` 
## Strict Eval Code
An eval code is strict eval code if it beings with a Directive Prologue that contains the `"use strict";` Directive or a direct call to the eval function that is contained in strict mode code.

## Strict Function Code
A function declaration, function expression or property assignment can be made into strict mode if it beings with a Directive prologue that contains the `"use strict";` directive

```
function TestStrict(){
    "use strict";
    console.log("I am strict!");
}
```
For the numerous features it has, ECMAScript clearly states rules on how the `strict mode` should treat certain cases. We will take note of them as and when we explore those features. 


# Primitives
A primitive value in JavaScript is a  [datum](https://en.wiktionary.org/wiki/datum) that is represented directly at the lowest level of the language implementation.

Basically if the datatype of something in the memory is not Object it has to be either of the following type:
- **Undefined **: when used as a *value* it is used when a variable is not assigned any value. As a datatype its the type of a variable whose sole value is `undefined`
```
var a = undefined;
``` 

- **Null **: This represents the *intentional* absence of an object value. As a datatype it is the type of a variable whose sole value is `null` value

```
var a  = null;
``` 

- **Boolean **: As value it can be either `true` or `false`, as a datatype it is type of a variable whose value is either `true` or `false`. 
```
var a = true;
var b = false;
``` 
- **String** : A finite ordered sequence of 0 or more 16-bit unsigned integer.  Each element(16-bit unsigned integer) occupies a position in the sequence. The positions are indexed with non-negative integers starting from 0.

```
var mystring = "ABCDEF";
//position   -  0123456 
``` 

- **Number** : As a primitive value it corresponds to double-precision 64-bit binary format IEEE 754 value. As a datatype is the set of all number values, a special **"Not-a-Number"(NaN)**, **positive infinity(+∞)** and **negative infinity(−∞)**

```
var a = 8;
var b = 8.9;
var c = +0;//result of expression like 1*0
var d = -0;//result of expression like -1*0
var e = +Infinity;//result of expression like 1/0
var f = -Infinity;//result of expression like -1/0
``` 


# Natives or native objects
These are objects whose semantics are fully defined by the ECMAScript specification and not by the host environment(browser JavaScript engine or the NodeJS run-time environment) in which the script runs.
Some of these natives are present at very start of the ECMAScript program execution(for example the Number, String, Boolean, etc) while others get created during the course of the program execution.
The ones that are present from the very beginning of the program execution are called as **built-in objects**. 

Apart from **Native and built-in objects** there are some objects called as **host object**  which are supplied by the host environment(browser JavaScript engine or the NodeJS run-time environment) like the `window` global variable supplied to us by the browser, or the  [global](https://nodejs.org/api/globals.html#globals_global) of NodeJS.

Let's see some built-in object that ECMAScript provides us:

- **Boolean Object** : A member of `Object` type that is an instance of the built-in `Boolean()` constructor,  created by using the `new` keyword on this constructor and passing a Boolean value as argument, which results in an object with an *internal property* whose value is the Boolean value.

```
var boo = new Boolean(true)
console.log(boo);//Output [Boolean: true]
``` 
- **String Object** : Same as Boolean but the `String()` constructor is called with a String value instead.

- **Number Object** : Same as Boolean but the `Number()` constructor is called with a Number value instead.

This sums up the basic of Natives and Primitives. We shall discuss in depth the built-in objects and the properties they carry in future posts.

For now let's dig into the Object Type a bit more in the next post.

References:

 [ecma-international.org/ecma-262/5.1/](ecma-international.org/ecma-262/5.1/)
 