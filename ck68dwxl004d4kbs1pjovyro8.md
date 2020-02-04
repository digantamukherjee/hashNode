## 7. The Object Type

In the  [last post](https://diganta.hashnode.dev/7-the-strict-mode-natives-and-primitives-ck5u2hhw407sjqks13emqetfs) we got to know about the *strict mode, natives and primitives* of JavaScript. Now lets understand in detail about a very important *type* in JavaScript which basically forms the base of this language called the *Object* type.

An `Object` in JavaScript is a collection of different kinds of properties. These properties can be of the following types:

- **A named data property**: this associates a name with a value and a set of *Boolean * attributes(we will see these shortly)

- **A named accessor property**: this associates a name with up to 2 functions and a set of Boolean attributes. Functions are either to *store or retrieve* the value associated with the property.(kind of like getter/setter)

- **An internal property**: it neither has a name nor is accessible. As the name suggests it is for internal use only.

Now each of the above mentioned types of properties have a set of it's own attributes, kind of like *property of a property*.  They are also commonly known as *property descriptors*. Let's see them:

## Attributes of a Named Data Property
- `[[Value]]` : (**Type:** It can be of any JavaScript type e.g. String, Boolean, Number, Object, etc) This is the value returned when the property is read by any program.
- `[[Writable]]` : (**Type:**  Boolean) Its a flag that determines whether the `[[Value]]` property can be modified using `[[Put]]` method. If set to `false` it will not succeed.
- `[[Enumerable]]`: (**Type:**  Boolean) This flag determines whether this property is visible/enumerated when the `Object`'s properties are accessed using a `for-in` loop.
- `[[Configurable]]`: (**Type:**  Boolean) This flag determines whether this property can be deleted from the Object or if any attributes of this property(other than the `[[Value]]` attribute) can be changed or if the property can be changed to be an accessor property.

Let's see some code in action:

```
//first let's create an Object using the Object constructor function
var obj = Object.create(null);//here we are setting its prototype to null explicitly
console.log(JSON.stringify(obj));//{}

//now we will add a property to "obj" object using some built-in methods of the Object constructor function
//We will see these functions later in details
Object.defineProperty(obj, "prop1", {
	value: 10,
	writable: true,
	enumerable: true,
	configurable: true
});
console.log(JSON.stringify(obj));//{"prop1":10} JSON.stringify works in a fashion similar to for-in

//let's change it's value first
obj.prop1 = "Hello";
console.log(obj.prop1);//Hello - it works and we are able to change the value to anything we want

//let's see if "prop1" can be accessed in a for-in loop
for(val in obj){
	console.log(val + " is " + obj[val]); // prop1 is Hello - this works too!
}//

//let's print the attributes/property descriptors
console.log(Object.getOwnPropertyDescriptors(obj, "prop1"));
/* output
{ prop1:
	{ 
		value: 'Hello',
		writable: true,
		enumerable: true,
		configurable: true 
	} 
}
*/
``` 
Here all the 3 attributes of the *Named Data Property* are set to true, which means we can change its value to any other value we want(`writable`),access it in the `for-in` loop(`enumerable`), delete the property or change its attributes or make it an accessor property. Now let us change the attributes and make them `false`:

```
//now let's override the attributes of the property and make them false
Object.defineProperty(obj, "prop1", {
	writable: false,
	enumerable: false,
	configurable: false
});

//let's print the object "obj" and see if
console.log(JSON.stringify(obj)); // {} this is correct as we have set enumerable to false

//and the actual for-in 
for(val in obj){
	console.log(val + " is " + obj[val]); // no output on screen as no enumerable property was found on the object
}

//now let's try to change the value of "prop1"
obj.prop1 = "Ciao"; // will fail
console.log(Object.getOwnPropertyDescriptors(obj, "prop1"));
/* output
{ prop1:
	{ 
		value: 'Hello',
		writable: false,
		enumerable: false,
		configurable: false 
	} 
}
*/

//now let's try to delete this property
delete obj.prop1; // will fail
console.log(Object.getOwnPropertyDescriptors(obj, "prop1"));
/* output
{ prop1:
	{ 
		value: 'Hello',
		writable: false,
		enumerable: false,
		configurable: false 
	} 
}
*/
``` 
 2 things to note here are are:
- If we redefine an existing property only attributes we specify newly gets changed. In the above example the `[[Value]]` attribute did not change.
- `Object.getOwnPropertyDescriptors` can access non-enumerable properties also.

At this stage, will the following code work now? Food for thought!

```
Object.defineProperty(obj, "prop1", {
	writable: true,
	enumerable: false,
	configurable: false
});
``` 

## Attributes of a Named Accessor Property
- `[[Get]]` : (**Type** : Object or `undefined`) If it's an object it must be a Function object. This special function's internal method `[[Call]]` is called using an empty arguments list to return the property value each time a get access of the property is performed.
- `[[Set]]` : (**Type** : Object or `undefined`). Similar to `[[Get]]`. Only this called with an assignment value.
- `[[Enumerable]]` : (**Type:**  Boolean) Flag which if `true` allows the property to be listed using for-in loop.
- `[[Configurable]]` : (**Type:**  Boolean) Flag which if `false` does not allow deletion of the property, changing its attributes or redefining it as a data property.
Let us see some code:

```
//let us create an object using literal notation
var obj = {
	num: 1,
}
console.log(JSON.stringify(obj)); // { num: 1 }

//let us define an accessor property "myNum" with both [[Get]] and [[Set]] attributes
Object.defineProperty(obj, "myNum", {
	enumerable: true,
	configurable: true,
	get(){
		return this.num;
	},
	set(num){
		this.num = num;
	}
});
console.log(JSON.stringify(obj));//  {"num":1,"myNum":1}
//let us modify "num" using the set() attribute of property "myNum"
console.log(obj.myNum);// 1
obj.myNum = 33;
//let us print "num" using the get() attribute of property "myNum"
console.log(obj.myNum);// 33 - this proves "myNum" is able to access and modify the "num" property
//let us change the num property directly and then access it using the get() of "myNum"
obj.num = 55;
console.log(obj.myNum);// 55 - this works too!

for(val in obj){
	console.log(val + " is " + obj[val]);
}
/*
Output
num is 55
myNum is 55
*/
``` 
The default values of these properties are as below:

- `[[Value]]` - undefined
- `[[Get]]` - undefined
- `[[Set]]` - undefined
- `[[Writable]]` - false
- `[[Enumerable]]` - false
- `[[Configurable]]` - false

## Internal Properties of Objects

These are not part of the JavaScript language and cannot be operated or called upon directly, but if any algorithm in the underlying implementation of the language uses these properties and the property implementation of object does not have such an internal property a `TypeError` us thrown. These properties are internally used by the JavaScript implementations to maintain the state of `Objects` created during the execution.


- `[[Prototype]]` - (**Value:** `Object` or `null`). The prototype of this object. Usually the *JavaScript run-time implementations* provide access to this property by adding a `__proto__` property to objects.
- `[[Class]]` - (**Value:**  `String`). Refers to the *classification* of the object.
- `[[Extensible]]` - (**Value:** `Boolean`) If true *own* properties may be added to the object.
- `[[Get]]` (propertyName)- (**Value:** any) Returns value of a named property
- `[[GetOwnProperty]]`(propertyName) - (**Value:** `undefined` or the *Property descriptor* of the *propertyName*) If object has *own property* called *propertyName* it's property descriptor is returned else `undefined` is returned.
- `[[GetProperty]]`(propertyName) - (**Value:** `undefined` or the *Property descriptor* of the *propertyName*) Returns the property descriptor of the property of the object if present, else returns `undefined`.
- `[[Put]]` (propertyName, any, Boolean) - Sets the specified named property to the value of the second parameter. The flag controls failure handling.
- `[[CanPut]]` (propertyName)  -  (**Value:** `Boolean`) Returns whether `[[Put]]` can be performed on the said property.
- `[[HasProperty]]` - (**Value:** `Boolean`) Returns whether the said property already exists on the object.
- `[[Delete]]` (propertyName, Boolean) - Removes the said property. Additional flag to control failure handling.
- `[[DefaultValue]]` - (**Value:** (Hint) → primitive) Returns a default value of  object.
- `[[DefineOwnProperty]]`(propertyName, PropertyDescriptor, Boolean) - Creates or alters own named property to have the said PropertyDescriptor. Additional flag to control failure handling.

### Some more Internal properties for Specific Objects
- `[[PrimitiveValue]]` - (**Value:** primitive) Only present for built-in `Boolean`, `String`, `Number` and `Date` objects. Represents the internal state information of these objects.
- `[[Construct]]` - (**Value:** object) This create a new object. It is invoked when the `new` operator is used. **Objects that implement this internal method are called as constructors.**
- `[[Call]]` - Executes code associated with the object. Invoked via a function call expression. **Objects that implement this internal method are callable. Only callable objects that are host objects may return Reference values.**
- `[[HasInstance]]` - (**Value:** `Boolean`) Only implemented by Functions. Returns a Boolean value indicating whether the argument is likely an Object that was constructed by this object. 
- `[[Scope]]` - (**Value:** Lexical Environment) **The environment in which the function object gets executed. Only Function objects implement this property.**
- `[[FormalParameters]]` - (**Value:** List of Strings) Only function objects implement this. It is a list of strings of identifiers.
- `[[Code]]` - Only function objects implement this. Returns the code of a function object.
- `[[TargetFunction]]` - (**Value:** `Object`) Only objects created from `Function.prototype.bind` method have this property.
- `[[BoundThis]]` - (**Value:** any) The pre-bound this value of a function Object created using the standard built-in Function.prototype.bind method. Only ECMAScript objects created using Function.prototype.bind have a [[BoundThis]] internal property.
- `[BoundArguments]]` - (**Value:** List of any) The pre-bound argument values of a function Object created using `Function.prototype.bind` method.
- `[[Match]]`(String, index) - Only `RegExp` implements this out of all built-in JavaScript object. It test for a regular expression match and returns the result.
- `[[ParameterMap]]` - (**Value:** `Object`) maps the properties of arguments objects and formal parameters. Only arguments objects implement this. 

So, this sums up the various properties a JavaScript object can have as per the EcmaScript specification. Additionally some run-time environments can provide access to some extra properties outside the specification scope.

Now that we have some idea about how an `Object` type's structure might look like let move on and explore *The Reference Specification Type* in our next post.