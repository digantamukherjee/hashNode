## 14. Built-in Objects of JavaScript/ECMAScript -Part I

In our  [last post](https://diganta.hashnode.dev/13-operators-in-javascript-ck6v1zyik04l9dfs1utgwtceb)  we got to know about the various operator that JavaScript support. Let us go back to the Specification and see in depth about the built-in objects and their properties. In this post we will discuss the **Global** object and **Object** object

# The Global Object #
- This is the object that is created when the JavaScript program execution is 
   about to be started the engine starts to enter any execution context.
- The properties of the *global object* have the attributes `{[[writable]]:true, [[Enumerable]]:false, [[Configurable]]:true}`.
- This object does not have a `[[Construct]]` internal property, which means 
   we cannot use the `new` keyword on this object to use it as a constructor.
- This object does not have a `[[Call]]` method on it. It is not *callable*. It 
   cannot be invoked as a function.
- The values of `[[Prototype]]` and `[[Class]]` are implementation dependent.
- In addition the host implementing the specification can add more properties to the global object. For example the `window` property of the global objects in the  [HTML Document Object Model(DOM)](https://www.w3.org/TR/2018/WD-html53-20181018/browsers.html#the-window-object)  is the global object itself.

### Value Properties of the Global Object :  ###
The following values are defined on the global object with attributes `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`

a) Nan  
b) Infinity  
c) undefined

### Function Properties of the Global Object :  ###
   **a) eval (x) **  
Evaluate or in short `eval` is the name of a function which takes a string as the input parameter and then executes those lines of code physically at the point where the eval statement was encountered.  
When this happens, depending on the way the eval statement is authored, the execution context selection for the statements encapsulated by the eval function differs. This if not fully understood can sometimes lead to a behavior that is not consistent with the rest of the JavaScript behavior. And this is primarily the reason you can see that in a lot of tutorials, discussions and blog posts developers are discouraged from using the eval functions citing security concerns.  
Now usage of `eval` differs if it is called as a *direct call* or *indirect call* and whether in *strict* mode or not. Examples: 

1. Direct call to eval in non-strict mode
```
eval("var a = 9");
console.log(a);//9
``` 
2. Direct call to eval in Strict mode
```
"use strict";
eval("var a = 9");
console.log(a);//ReferenceError: a is not defined
``` 
3. Indirect call to eval in non-strict Mode
```
(1, eval)("var a = 9");
console.log(a);//9
```
4. Indirect call to eval in Strict Mode
```
"use strict";
(1, eval)("var a = 9");
console.log(a);//9
```
In the above examples, the last piece of code does not honor the *strict mode* because it is not a direct call and hence not a  [Strict Eval call](https://www.ecma-international.org/ecma-262/5.1/#sec-10.1.1).  

**b) parseInt(string, radix) **  
It converts the given *string* into integer value of the base *radix*.  
Leading white space of string are is ignored.  
If radix is `undefined` or 0, its assumed to be **10**, unless the string starts with `0x` or `0X` in which case radix is set to **16**.  
  
**c) parseFloat(string) **  
It produces a *Number* value. In both `parseInt` and `parseFloat` the leading portion as numeric value and ignore the rest of the string where it encounters any non-numeric character.  
  
**d) isNaN(number)**  
Returns `true` if the argument evaluates to `NaN`, else returns `false`
  
**e) isFinite(number)**  
Returns `false` if argument evaluates to `NaN`, `+∞`, or `−∞`, otherwise returns `true`.  
  
### URI Handling Function Properties of the Global Object :  ###  
There are 4 functions that help to encode/decode URI and its components.  
1. encodeURI
2. decodeURI
3. encodeURIComponent
4. decodeURIComponent
  
`encodeURI` and `decodeURI` work with the complete URI and assume any reserved characters will be having a special meaning and hence do not encode them.  
`encodeURIComponent` and `decodeURIComponent` however work with individual component parts of the URI and will treat reserved characters as text and hence encode it.  
  
### Constructor Properties of the Global Object :  ###  
The Global object exposes the constructor functions of all the built-in functions of JavaScript like `Object()`, `Function()`, `Array()`, `String()`, `Boolean()`, `Number()`, `Date()`, `RegExp()`, `Error()`, `EvalError()`, `RangeError()`, `ReferenceError()`, `SyntaxError()`, `TypeError()` and `URIError()`.  
  
### Other Properties of the Global Object :  ###  
These include built in helper Objects like `Math()` and `JSON`.  
  
# The Object object#  
The JavaScript Object object can be used as either a *Function* or  with the `new` keyword(here the constructor creates the object) and the behavior is same for both of  these invocations.

Beside the internal properties and the length property the Object constructor has the following properties:    
### Properties of the Object Constructor ###  
**a) Object.prototype**  
**b) Object.getPrototypeOf (O)** - Return the internal [Pprototype]] property of the object passed as argument(O).  
**c) Object.getOwnPropertyDescriptor(O,P)** - Returns the property descriptors of the property **P** of the object **O**.  
**d) Object.getOwnPropertyNames** - Returns an array of *named own properties* of the object.  
**e) Object.create ( O [, Properties] )** - Creates a new object from the prototype object **O** and adds properties  present in the **[, Properties]** object.  
**f) Object.defineProperty ( O, P, Attributes )** - Defines a new property **P** with attributes defined by the **Attributes** object on an object **O**.  
**g) Object.defineProperties( O, Properties )** - Similar to previous one, just we can pass multiple properties using the **Properties** object.  
**h) Object.seal ( O )** - This sets the `[[Configurable]]` attribute of all the properties of the object to `false` and renders the object non-extensible by setting the `[[Extensible]]` property to `false`.  
**i) Object.freeze ( O )** - This sets the `[[Configurable]]` and `[[Writable]]` attribute of all the properties of the object to `false` and renders the object non-extensible by setting the `[[Extensible]]` property to `false`.  
**j) Object.preventExtensions ( O )** -  Renders the object non-extensible by setting the `[[Extensible]]` property to `false`.  
**k) Object.isSealed( O )** - Returns true if all the conditions of `Object.seal( O )` are met else returns `false`.  
**l) Object.isFrozen ( O )** - Returns true if all the conditions of `Object.freeze ( O )` are met else returns `false`.   
**m) Object.isExtensible ( O )** - Returns the value of [[Extensible]] internal property of the object.  
**n) Object.keys ( O )** - Returns an array of the keys of the object's properties.  
  
### Properties of the Object Prototype Object ###  
**a) Object.prototype.constructor** - The evaluates to the build in object-constructor.  
**b) Object.prototype.toString ( )** - Converts the prototype to string and displays as result of concatenating the three Strings "[object ", class, and "]"  
**c) Object.prototype.toLocaleString ( )** - Same as above `toString()` implementation. This function is provided to give all Objects a generic toLocaleString interface, even though not all may use it. Currently, Array, Number, and Date provide their own locale-sensitive toLocaleString methods.  
**d) Object.prototype.valueOf ( )** - The valueOf() method returns the primitive value of the specified object.  
**e) Object.prototype.hasOwnProperty (V)** - This check for existence of a own property of the object and not as part of the prototype chain.  
**f) Object.prototype.isPrototypeOf (V)** - The isPrototypeOf() method checks if an object exists in another object's prototype chain..  
**g) Object.prototype.propertyIsEnumerable (V)** - This method does not consider objects in the prototype chain.   

### Properties of Object Instances ###  
Instance do not have any special properties apart form what they inherit form the Object prototype object.
  
  
We shall continue with in-depth study of built in objects in the  [next post](https://diganta.hashnode.dev/15-built-in-objects-of-javascriptecmascript-part-ii-ck6xhd24g05d8dfs1nne8gf6s) .  

Reference:  
[ECMA](https://www.ecma-international.org/ecma-262/5.1/#sec-15)  
[https://stackoverflow.com/questions/19357978/indirect-eval-call-in-strict-mode](https://stackoverflow.com/questions/19357978/indirect-eval-call-in-strict-mode) 