## 6. Prototype chain, Object.create() and more..

In my  [last ](https://diganta.hashnode.dev/5-prototype-based-inheritance-in-javascript-ck5psgyjw06bdqps1wl6rsdnf) post we drilled down the prototype chain until we reached  [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) and finally hit the `null`. Now let's reverse our journey and say we have an object which has 2 levels of inheritance above it. So what happens when we try to access it's parent's or grandparent's properties? How do we know whether any property mentioned belongs to the object or it parent or grandparent?
Let's see some code:

```
function MyPseudoGrandParentClass(myProperty){
}

function MyPseudoParentClass(myProperty){
}

function MyPseudoChildClass(myProperty){
}

//create the prototype chain or inheritance
MyPseudoChildClass.prototype.__proto__ = MyPseudoParentClass.prototype;
MyPseudoParentClass.prototype.__proto__ = MyPseudoGrandParentClass.prototype;


//Let's add some property to the grand parent and parent 
MyPseudoGrandParentClass.prototype.sayHelloGP = function(){
	console.log("Hello Grandpa and Grandma!");
}
MyPseudoParentClass.prototype.sayHelloP = function(){
	console.log("Hello Mom and Dad!");
}

var myPseudoChildClassObject = new MyPseudoChildClass();

//Can child access the properties of parent and grand parent?
myPseudoChildClassObject.sayHelloGP();//Output Hello Grandpa and Grandma!
myPseudoChildClassObject.sayHelloP();//Output Hello Mom and Dad!
``` 
What JavaScript does here is it traverses the prototype chain until it finds the *callable *properties sayHelloGP and sayHelloP and then executes them.
In case it fails to find them, it results in `TypeError`.

What do you think will happen if I add `sayHelloP` as a property on the child class itself.

```
function MyPseudoGrandParentClass(myProperty){
}

function MyPseudoParentClass(myProperty){
}

function MyPseudoChildClass(myProperty){
	this.sayHelloP = function(){
		console.log("Not calling parent!");
	}
}

//create the prototype chain or inheritance
MyPseudoChildClass.prototype.__proto__ = MyPseudoParentClass.prototype;
MyPseudoParentClass.prototype.__proto__ = MyPseudoGrandParentClass.prototype;


//Let's add some property to the grand parent and parent 
MyPseudoGrandParentClass.prototype.sayHelloGP = function(){
	console.log("Hello Grandpa and Grandma!");
}
MyPseudoParentClass.prototype.sayHelloP = function(){
	console.log("Hello Mom and Dad!");
}

var myPseudoChildClassObject = new MyPseudoChildClass();

//Can child access the properties of parent and grand parent?
myPseudoChildClassObject.sayHelloGP();//Output Hello Grandpa and Grandma!
myPseudoChildClassObject.sayHelloP();//Output Not calling parent!
``` 
Obviously preference will be given to the child class property. This is the result of something called **Scope** which we will discuss in detail later. So in this situation if we specifically want to call the parent's method how to do so? We can do that by accessing the prototype chain until we reach the parent and then call the method on that:

```
myPseudoChildClassObject.__proto__.sayHelloP();//Output Hello Mom and Dad!
``` 


Now how do we figure out if we are able to access any property on the child class, whether it belongs to the child class or is it an inherited property of it's parent chain? `Object.prototype.hasOwnProperty` is a method on the Object class that helps us to figure that out.

```
if(myPseudoChildClassObject.hasOwnProperty("sayHelloGP")){
	myPseudoChildClassObject.sayHelloGP();
}else{
	console.log("property not found on object");//Output property not found on object
}
``` 
Now that we have the basic understanding of traversing the chain both ways, let's move on to the last method of object creation i.e. `Object.create()`

### Object.create()
Syntax: `Object.create ( O [, Properties] )`
How does this work?  [EcmaScript ](https://www.ecma-international.org/ecma-262/5.1/#sec-15.2.3.5) specifies the following algorithm for this method:
1. If type of `O` is null or not object then throw `TypeError` exception
2. Create an object(`obj`) as if created using the `new` keyword on the built-in `Object` function constructor. (`var obj = new Object();`)
3. Set the [[prototype]] internal property of `obj` to `O`. (`obj.__proto__ = O`)
4. If the optional arguments `[, Properties]` is not undefined then add own properties to `obj` as if by calling the built-in property of `Object`  `Object.defineProperties` with arguments `obj` and `Properties`.
5. Return `obj`

So now we have seen all the various ways in which an object in JavaScript can be created.

An important observation to make here is that creating a **private or closed scope** is possible by using the constructor function method of object creation using the `new` keyword, but is somewhat difficult with literal notation or `Object.create()`.
A private or closed scope can be created defining properties on the constructor function using the `var` keyword instead of the `this` keyword.

```
function ObjMain(){
	var internal = "intternalVar";
	this.publicVar = "publicVar";
	this.revealInternal = function(){
		return internal;
	}
	
}

var newObj = new ObjMain();
console.log(newObj);//Output ObjMain { publicVar: 'publicVar', revealInternal: [Function] }
console.log(newObj.internal);//Output undefined
console.log(newObj.publicVar);//Output publicVar
console.log(newObj.revealInternal());//Output intternalVar
``` 
If we look at the new object created the property `internal` is missing. `publicVar ` is present. We can define a function named `revealInternal ` which is similar to what we call a *getter* method in Classical inheritance terminology to reveal the value that is part of the *private or closed scope* of the function constructor. If you have heard the term **Closure** in the context of JavaScript, what we saw just now was, well  **Closure** in action. But we shall deal with the intricacies of **Scope** and **Closure** in later posts.

In our  [next post](https://diganta.hashnode.dev/7-the-strict-mode-natives-and-primitives-ck5u2hhw407sjqks13emqetfs)  we will define the basic datatypes available in JavaScript.


**References**

 [http://ecma-international.org/ecma-262/5.1/#sec-15.2.3.5](http://ecma-international.org/ecma-262/5.1/#sec-15.2.3.5) 