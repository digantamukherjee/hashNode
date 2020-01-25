## 2. ECMAScript Overview

## What is was originally?
Originally ECMAScript was developed to be a Web Scripting Language which would be used to make a web page more lively by manupulating, customizing and automating the functionalities of the existing system in which it runs. 

## And as of today...
ECMAScript is an object-oriented programming language for performing computations and manipulating computational objects within a host environment.

In the core of a Web browser is a browser engine and an ECMAScript engine(mostly JavaScript engine) which provides the host environment for execution of the JavaScript code.

For example the Google Chrome browser uses the  [Blink](https://www.chromium.org/blink)  rendering engine and the  [V8 ](https://v8.dev/)  JavaScript engine.

By using JavaScript at the client side the client-server architecture has evolved greatly as the computation power could now be distributed on both client and server.

## A brief of data-types used in JavaScript(JS)
The various data-types used in JS can be categorized as follows
- **The Primitives**: ```Undefined```, ```Null```, ```Boolean```, ```Number```, ```String```
- **Objects**: A collection of properties where each *property* has *attributes* that define how the property can be used. These properties can hold primitive values, other objects or functions.
- **Functions**: JS functions are also objects. They differ from other objects that contain data because functions are **callable** object, meaning they can be executed or called from other parts of the program.
- **Method**: It is nothing but a name given to functions that are present as a property of an object.

## Special built-in objects
There are some special objects that include the **global** object, the **Object** object, the **Function** object, the **Array** object, **String** object, **Boolean** object, **Number** object, **Math** object, **Date** object, **RegExp** object, **JSON** object and some error objects like **Error**, **EvalError**, **ReferenceError**, **TypeError**, **RangeError**, **SyntaxError** and **URIError**

## Operators: 
JS includes operators like Unary, Binary, Ternary, Additive, Multiplicative, Bitwise, Relational, Equality, Bitwise Shift, Binary Bitwise, Binary Logical, Assignment and Comma operators.

A few other interesting JS features are which we will experience in detail later on are:

- Its syntax is intentionally made similar to Java for ease of use

-  It is loosely typed language which mean we need not declare the type of a variable during its declaration.
- A function can be called before its declaration appears textually.
   
```
a();

function a(){
	console.log("Seems to be declared after its called");
}
``` 
**This works in JS world!!! We will see in detail later why this works.**

This was a little bit of overview of JavaScript. In the next post we shall dive into detailed features of the language. Let's start with  [Objects](https://diganta.hashnode.dev/3-about-js-objects-ck5o6dqbb05czqks1tmq6wkl9) 


References:

 [https://www.ecma-international.org/ecma-262/5.1](https://www.ecma-international.org/ecma-262/5.1) 

 [https://en.wikipedia.org/wiki/Browser_engine](https://en.wikipedia.org/wiki/Browser_engine)

 

