## 3. About JS Objects...

As of ES5.1 *classes* did not exist similar to other programming languages like *C++ or Java*.

**So how do we create objects?**
- Via literals 

```
var obj = {};
obj.myProperty = "Hello literal object!"
console.log(obj.myProperty);
``` 
- Via constructor functions

Its simply a JavaScript function.
```
function MyPseudoClass(){
    this.myProperty = "Hello World!"
}
var myPseudoClassObject = new MyPseudoClass();
console.log(myPseudoClassObject.myProperty);
``` 
Each constructor function has a property named "prototype"(which JavaScript adds under the hood the moment the function is created) which is used to implement **prototype-based inheritance** and **shared properties**

Objects are created using the **new ** keyword.  Omitting the new keyword has its consequences that depends on the constructor. For example ```Date()``` produces a date string and ```new Date()``` produces a date object.

Whenever we create an object of a constructor function using the new keyword, JavaScript automatically adds a property named as ```__proto__``` to the object and this property points to the ```prototype``` property of the constructor function.


![constructor_function_object_relation_diagram.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1579633613457/PA1sWJ6Jr.png)

Now that we have defined what constructor functions are and how are new object created from them related to these constructor functions we can revisit object creation via literals and see actually it uses the function constructor way of object creation under the hood. How? Here's how...

If you remember in the  [previous ](https://diganta.hashnode.dev/2-ecmascript-overview-ck5fqhk1g02n5qps1dq0o72yv) post of this series we had mentioned some special built-in objects that JavaScript has, one of which was the `Object` object. So when JavaScript encounter the `{}` notation of creating objects, it internally calls the function constructor of the built-in `Object` function as `new Object()`. This creates a new instance of the `Object` function constructor and assigns its `__proto__` to `prototype` property of `Object` function constructor.



- Via Object.create

We shall explore this method of object creation separately in later post and then compare with the previous two methods we have seen just now.

In the  [next section](https://diganta.hashnode.dev/4-prototype-__proto__-and-inheritance-in-javascript-ck5oexqoh05fwqks1yae12w6t)  we will explore ```prototypes ``` and ```__proto__``` in detail to understand its significance in the language.

References:
 [https://www.ecma-international.org/ecma-262/5.1](https://www.ecma-international.org/ecma-262/5.1/) 