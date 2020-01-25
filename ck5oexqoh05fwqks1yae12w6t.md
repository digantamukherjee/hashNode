## 4. prototype, __proto__ and inheritance in JavaScript

In the  [last post](https://diganta.hashnode.dev/3-about-js-objects-ck5o6dqbb05czqks1tmq6wkl9) we mentioned a behavior of objects created when the `new` keyword is used on a constructor function, i.e. JavaScript adds a property called as `__proto__` which points to the `prototype` property of the constructor function. 

**Note:** Before *EcmaScript 2015 or ES6* there was no official way to access an object's internal `prototype` property. The formation of links for the purpose of inheritance was done using an internal property called `[[prototype]]`. It is modern browsers and JavaScript engines who provide the `__proto__` property to enable pointing to the constructor function's `prototype` object.

Now let's see some code that reveals what happens under the hood. Let's define a constructor function(or also called as function constructor) and see whether really it has a property called `prototype`

```
function MyPseudoClass(){
    this.myProperty = "Hello World!"
}
console.log(MyPseudoClass.prototype);//Output MyPseudoClass {}
``` 
Yes it does and its an object. Now let's create a new object using this function constructor and check its `__proto__` property.

```
var myPseudoClassObject = new MyPseudoClass();
console.log(myPseudoClassObject.myProperty);//Output "Hello World!"
console.log(myPseudoClassObject.__proto__);//Output MyPseudoClass {}
``` 
Yes `__proto__` and it also points to an object.
Are `MyPseudoClass.prototype` and `myPseudoClassObject.__proto__`  pointing to the same object? We can easily check this using the below equality comparison because *in JavaScript equality comparison of objects actually checks where these properties are pointing to in the memory*.

```
console.log(MyPseudoClass.prototype===myPseudoClassObject.__proto__);//true
``` 
Yes they are the same! 

![constructor_function_object_relation_diagram.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1579634490920/s9QcF6Fbl.png)
Now does the newly created object have a prototype property by any chance?

```
console.log(myPseudoClassObject.prototype);//undefined
``` 
No it does not! As per JavaScript its **not a constructor function** and so it does not have its own prototype. Can we add one explicitly?

```
myPseudoClassObject.prototype = {
	"language": "javascript"
};
console.log(myPseudoClassObject.prototype);//{ language: 'javascript' }
``` 
Yes we can and it remains as a property of `myPseudoClassObject` only.

In a  [previous](https://diganta.hashnode.dev/2-ecmascript-overview-ck5fqhk1g02n5qps1dq0o72yv) post I mentioned about another built-in object, the `Function` object, which acts as the parent of any constructor function we create. So based on what we have learnt about the `prototype` and `__proto__` relation, the `__proto__` of any **constructor function** should be pointing to the `prototype` property of the built-in `Function` object. Let's check if this is true:

```
console.log(MyPseudoClass.__proto__=== Function.prototype);//true
console.log(MyPseudoClass.__proto__);//[Function]
``` 
Yes it does! Now that we are aware of the relation of `prototype` and `__proto__` objects let us see what the internal structure of the `prototype` object.

**The prototype object**

The `prototype` object contains 2 properties of interest for us right now:

1.  `constructor`
2.  `__proto__`

The `constructor` is nothing but a reference to the very object to which this `prototype` belongs.
And the `__proto__` points to either any other constructor function's `prototye` or to the `prototype` of the built-in `Object` constructor function.

So it can be better visualized as below:

![prototype_proto_linksge.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1579642454942/pNAmalj-W.png)

And this is what gives rise to the concept of **prototype-based inheritance** when we start linking one constructor function's `__proto__` object to the `prototype` of another constructor function.

We shall explore the inheritance mechanism of JavaScript in the  [next ](https://diganta.hashnode.dev/5-prototype-based-inheritance-in-javascript-ck5psgyjw06bdqps1wl6rsdnf) post.

References:

 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)

 [https://hackernoon.com/understand-nodejs-javascript-object-inheritance-proto-prototype-class-9bd951700b29](https://hackernoon.com/understand-nodejs-javascript-object-inheritance-proto-prototype-class-9bd951700b29)  

 [https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes) 


