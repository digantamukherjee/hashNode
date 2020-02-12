## 11. this and call, bind and apply in JavaScript

Since the  [last post](https://diganta.hashnode.dev/10-lexical-environments-execution-context-scope-and-hoisting-in-javascript-ck6id2pdo00dmd9s1siz1jasg)  have now a better understanding of how the JavaScript interpreter behaves. There we observed how the execution context gets created with its state maintained in few reference like the *scope chain*, *lexical environment* and the *this* variable. We also understood the contents of the *scope chain* and the *lexical environment*. 

Now let's move on to understand fully about the **this** object that is part of the execution context.

# this
> `this` is an object whose value if used in the global context is always set to the global object and if used in the context of a function, it is set at the point when the function is called but before the actual execution of the function code. Inside a function call, the value of `this` is influenced by the parent scope in which the function is called and also the function syntax.

**The value of this is never static**. 

Function declaration/definition does not determine `this`. The point during the execution of the JavaScript program at which a function gets invoked is the point at which a value is assigned to `this`.

Now let's prove the above points using some code. 

## this in Global Context
We will start with the understanding `this` in the global execution context. For this we will create a variable outside a function using the var keyword. This should get attached to the global context. *Since we have claimed that the `this` in global context points to the global object, the new variable should be a property of `this`.*
And this would prove our first point. If we run the below code in the browser let's see the output:

```
// global scope
var name = "Jim";
console.log(name); // Jim
this.name = "Jack";
console.log(name); // Jack
``` 
** A word of caution: In a different JavaScript interpreter like Node JS the behavior of the above code will be different as `var <something>*` will be local to the  [module](https://nodejs.org/api/modules.html#modules_module) . See  [nodeJS documentation for more info](https://nodejs.org/api/globals.html#globals_global) **

## `this` in Function/Method Execution Context
As we had noted earlier, for the case of a function/Method execution context the value of `this` is assigned at the point the function is physically accessed or invoked or called. If we recall the difference of a **function** and a **method** then it will be easier to understand how `this` will be different for them.
A function written as part of the global execution context is called as a **function** whereas on which is a *property of an object* is called as a **method**. This subtle difference causes difference in `this`.
Let as assume the below function defined in the global scope:

```
//global context
function getThis(){
	return this;
}
console.log(getThis() === global);// true - this is for Node JS
console.log(getThis() === window);// true - this is for browsers
``` 
So for a function present in the global context and invoked in the global context (**without any additional semantics**), the **`this` points to the global object**.


> It is worthy to note that if the function body is expected to to be executed in `strict` mode, then the interpreter will not assign the global object reference to the `this` property of execution context. It should remain undefined. However some browsers including Google Chrome (till now seen till version 79.0.3945.130) has not implemented it correctly and it still return the `window` object which is incorrect and a bug. Since NodeJS shares the same JavaScript engine(v8) as 
Chrome, it has the same bug and will assign the `global` object to `this`

```
function test(){
	`use strict`
	return this;
}
console.log(test() === window);//false-- ideally for browsers, but Chrome prints "true"
console.log(test() === global);//false-- ideally for NodeJS but it prints "true"
``` 



*Function invocation in JavaScript should always be visualized as if it is called as a property of some object.* Then it will be easier to figure out the `this`. In this case the function, `getThis` though called using the syntax 'getThis()' , should be visualized as if called using the syntax `global.getThis()` or `window.getThis()`. So since this is part of the global context and no one has assigned any this value, interpreter assigns the default value of `global`(NodeJS)/`window`(browsers) to `this`. Actually in case of browsers `getThis` function actually gets attached to the `window` object. Again, in Node JS it's bit different from browsers owing to existence of modules. 

 The additional semantics that we mentioned might be able to alter the value of `this` are properties of `Function.prototype` known as `call`, `bind` and `apply`. We will see about them shortly.

Now if we invoke a method of an object, then the `this` will point to the object itself. Let's see some code:

```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(){
		console.log("hello " + this.name);
	},
	checkThis : function(){
		console.log(this === myObj);
	}
};

myObj.printName();//hello Gollum
myObj.checkThis();//true
``` 
So here the functions `printName()` and `checkThis()` are both invoked as a property of the object `myObj` and hence the `this` value points to the object `myObj` itself.


> If a function constructor is invoked with the `new` keyword, the `this` variable actually reference to the newly created object.

Now what if we do the below? What exactly happens in this case? Why is it unable to resolve the property `name` and prints it as `undefined`. Also why is `this` not pointing to `myObj` in this case?

```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(){
		console.log("hello " + this.name);
	},
	checkThis : function(){
		console.log(this === myObj);
	}
};

var someFunc1 = myObj.printName;
var someFunc2 = myObj.checkThis;

someFunc1();//hello undefined
someFunc2();//false
``` 
**The reason here we are not invoking methods `printName` and  `checkThis` of object `myObj` but independent functions in the global context which are copies of methods of object `myObj` i.e. `printName` and  `checkThis`**

This happens when the below lines get executed. Copies of those 2 methods are created and assigned to `someFunc1` and `someFunc2`.

```
var someFunc1 = myObj.printName;
var someFunc2 = myObj.checkThis;
``` 
So when `someFunc1` and `someFunc2` are invoked, they actually are invoked in the global context and since there is no other code present to modify their `this` value, interpreter assign the global object to `this`.


```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(){
		console.log("hello " + this.name);
	},
	checkThis : function(){
		console.log(this === myObj);
		console.log(this === global);
	}
};

myObj.printName();//hello Gollum
myObj.checkThis();//true false

var someFunc1 = myObj.printName;
var someFunc2 = myObj.checkThis;

someFunc1();//hello undefined
someFunc2();//false true
``` 
So how do we preserve the value of this or pass the correct this to the functions? We can use either of `call`, `bind` or `apply`.

# `bind`


> One ring to rule them all, one ring to find them, One ring to bring them all and in the darkness bind `this` :)

This is a property of the JavaScript `Function.prototype` object, which is used to set  the this value of a function object. It returns the function on which it is invoked with its `this` property set to the argument that is passed when `bind` is called. Let us see the code:

```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(){
		console.log("hello " + this.name);
	},
	checkThis : function(){
		console.log(this === myObj);
		console.log(this === global);
	}
};

var someFunc1 = myObj.printName.bind(myObj);
var someFunc2 = myObj.checkThis.bind(myObj);

someFunc1();//hello Gollum
someFunc2();//true false
``` 
So effectively bind is setting the `myObj` object as `this` on copies of `myObj.printName` and `myObj.checkThis` and then returns them to `someFunc1` and `someFunc2` respectively, so that when they are invoked even in the global context, interpreter finds out that the `this` of those functions are already defined and makes of the appropriate value.


> Bind can take additional optional parameters as well.

# `call`
This is a property of the JavaScript `Function.prototype` object, which is used to invoke the function immediately with a list of parameters passed to it as per some rule.

The rule is that the *first parameter* to call sets the value of `this` variable and any other additional parameters that needs to be passed on to the function should be comma-separated values(remember **C** for **C**all and **C**omma). Let's see some code:


```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(extraParam1, extraParam2){
		console.log("hello " + this.name);
		console.log(extraParam1 + " " + extraParam2);
	},
	checkThis : function(){
		console.log(this === myObj);
		console.log(this === global);
	}
};

var someFunc1 = myObj.printName;
var someFunc2 = myObj.checkThis;

someFunc1.call(myObj, "gollum", "gollum!");// hello Gollum , gollum gollum!
someFunc2.call(myObj);// true, false
``` 
And as discussed we are able to manipulate the value of `this` and at the same time pass on some extra parameters too.


# `apply`

This is a property of the JavaScript `Function.prototype` object, which is used to invoke the function immediately with a list of parameters passed to it as per some rule.

The rule is that the *first parameter* to call sets the value of `this` variable and any other additional parameters that needs to be passed on to the function should be provided as an array(remember **A** for **A**pply and **A**rray). Let's see some code:


```
//method execution in context of object
var myObj = {
	name : "Gollum",
	printName : function(extraParam1, extraParam2){
		console.log("hello " + this.name);
		console.log(extraParam1 + " " + extraParam2);
	},
	checkThis : function(){
		console.log(this === myObj);
		console.log(this === global);
	}
};

var someFunc1 = myObj.printName;
var someFunc2 = myObj.checkThis;

someFunc1.apply(myObj, ["gollum", "gollum!"]);// hello Gollum , gollum gollum!
someFunc2.apply(myObj);// true, false
``` 

The result is exactly the same as for `call`.

So now we have an even better idea how the interpreter finds the value of `this` in JavaScript. Also we have a better understanding of execution context, its components and how they behave, let us try to understand another very important concept called as **closures** in our next post.

References:

 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) 
