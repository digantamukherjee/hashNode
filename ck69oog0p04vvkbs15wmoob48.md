## 9. The Reference Specification Type and Type Conversions in JavaScript

In our  [last post](https://diganta.hashnode.dev/8-the-object-type-ck68dwxl004d4kbs1pjovyro8) we got to understand the constituent properties of the `Object` type of JavaScript.
Now let us have a look at the other types defined in the specification.

# Reference Specification Type
A **Reference** is a resolved name binding.

It is made up of 3 components:
- Base Value: (**Type:** `undefined`, `Object`, `Number`, `String`, `Boolean`, Environment Record). A base value of `undefined` means the *reference* could not be resolved to a binding.
- Referenced name: (**Type:** `String`) The name given to the reference.
- Strict reference flag: (**Type:** `Boolean`)

# Type Conversions in JavaScript
The JavaScript runtime system performs automatic **type conversions** as needed. 

Conversion can be done to primitive types as per the following rules:
- **ToBoolean**

This converts the argument to Boolean type.

```
//falsy values in JavaScript
console.log(Boolean(undefined));// false
console.log(Boolean(null));// false
console.log(Boolean(0));// false
console.log(Boolean(+0));// false
console.log(Boolean(-0));// false
console.log(Boolean(""));// false
console.log(Boolean(false));// false

//truthy values in JavaScript
console.log(Boolean(true));// true
console.log(Boolean("Hello"));// true
console.log(Boolean(4));// true
console.log(Boolean(+Infinity));// true
console.log(Boolean(-Infinity));// true
console.log(Boolean("0"));// true
console.log(Boolean({}));// true
``` 
- **ToNumber**
This converts the argument to Number type.

```
console.log(Number(undefined));// NaN
console.log(Number(null));// 0
console.log(Number(0));// 0
console.log(Number(+0));// 0
console.log(Number(-0));// -0
console.log(Number(""));// 0
console.log(Number(true));// 1
console.log(Number("Hello"));// NaN
console.log(Number(4));// 4
console.log(Number(+Infinity));// Infinity
console.log(Number(-Infinity));// -Infinity
console.log(Number("0"));// 0
console.log(Number({}));// NaN
``` 
- **ToString**
This converts the argument to String type

```
console.log(String(undefined));// undefined
console.log(String(null));// null
console.log(String(0));// 0
console.log(String(+0));// 0
console.log(String(-0));// 0
console.log(String(""));// <empty string>
console.log(String(true));// true
console.log(String("Hello"));// Hello
console.log(String(4));// 4
console.log(String(+Infinity));// Infinity
console.log(String(-Infinity));// -Infinity
console.log(String("0"));// 0
console.log(String({}));// [object Object]
``` 
- **ToObject**

This converts the argument to String type. Trying to convert `undefined` and `null` to Object type will result in `TypeError` exception.

An interesting thing to note about JavaScript is a numeric character within quotes gets converted to `Number` type in the below scenario:

```
var varA = "8"+"0";
console.log(varA); //80
console.log("typeof varA is " + (typeof varA));//typeof varA is string 
// this is a way of string concatenation in Javascript

var varB = +"8" + +"0"
console.log(varB);//8
console.log("typeof varB is " + (typeof varB));//typeof varB is number

varC = "88";
console.log(typeof +varC);//number
``` 
Also we have spoke about the *Reference Specification Type* , now it is important to note that :

*Primitive types in JavaScript are always passed by value*

*Objects types in JavaScript are always passed by reference*

Let's see examples to prove this:

```
var a = 20;
var b = a;
console.log(a);// 20
console.log(b);// 20
a= 90;
console.log(a);// 90
console.log(b);// 20
``` 
Here when the assignment `var b = a;` was done, JavaScript executed this code in the below steps:
1. Create a variable reference.
2. Add the named reference of the variable created in step 1 as `b`
3. **Copy the value** of variable referenced by the named reference `a` to the variable with named reference `b`

Let's see another example of primitives:

```
var c = 77;// 77
console.log(c);
function ChangeNum(x){
	x = 88;
}
ChangeNum(c);
console.log(c);// 77
``` 
This further proves our point. Now let us try it on Object and Object properties:

```
var objB = objA;
console.log(objA);//{ prop1: 'aProp' }
console.log(objB);//{ prop1: 'aProp' }
//now we will change "prop1" property of object referenced by objB and see if objA also gets changed
objB.prop1 = "bProp";
console.log(objA);//{ prop1: 'bProp' }
console.log(objB);//{ prop1: 'bProp' }
``` 
This confirms that objects get passed around by reference. Now let us assign a different object to objA. In this case will `objB` loose the reference to the previous object? Let's see

```
objA = {
	differentProp1 : "differentA"
}
console.log(objA);//{ differentProp1: 'differentA' }
console.log(objB);//{ prop1: 'bProp' }
``` 
**No it does not**. It is still pointing to the same memory location as it was earlier. but `objA` now points to a different location in the memory because the `=` operator changes the reference named `objA` to a new location in the memory.

```
function ChangeObjectProp(o){
	o.prop1 = "changedProp";
}
ChangeObjectProp(objB);
console.log(objB);//{ prop1: 'changedProp' }
``` 
Here the formal parameter to the function `ChangeObjectProp` holds the reference same as what `objB` holds. And because of this the original object got modified which again confirms that the object was passed by reference.

Now we have seen the Reference Specification type and how type conversions will behave in the world of JavaScript, we will move on to a core concept of Lexical Environment and Scope in our next post.