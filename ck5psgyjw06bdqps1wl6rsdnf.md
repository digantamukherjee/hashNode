## 5. Prototype based inheritance in JavaScript

As we have seen in our  [last post](https://diganta.hashnode.dev/4-prototype-__proto__-and-inheritance-in-javascript-ck5oexqoh05fwqks1yae12w6t) the association of `prototype` and `__proto__` objects creates the interesting phenomenon of *prototype based inheritance* in JavaScript. *EcmaScript *has specified some rules for controlling the behavior of `[[prototype]]` object.They are:

- All objects have an internal property called `[[prototype]]` whose value is either `null` or an object and it will be used for implementing inheritance.

- Every **prototype chain** must have a finite length, i.e. if we recursively start accessing the `[[prototype]]` object of an object, this internal property must eventually terminate at `null`.

So how does the inheritance look like? Let's visualize it:
For this let us take the stages of human evolution as our inheritance chain: 

**Australopithecus afarensis->Homo habilis->Homo erectus->Homo neanderthalensis->Homo sapiens**

The inheritance would look something like this:

![prototype_chain_inheritance.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1579722214146/3cXqLtaP7.png)

So here:

- All constructor function’s `__proto__` property points to the `prototype` property of the JavaScript `Function` constructor function

- All constructor function’s `prototype` property points to an object

- The `constructor` property of the `prototype` object of a constructor function points to the constructor function itself

- The `__proto__` property of the `prototype` object of a constructor function points either to it’s parent constructor function’s `prototype` property or to `prototype` property of the JavaScript `Object` constructor function. And this is the mechanism used in JavaScript to inherit methods of its parent objects.
Ultimately the `__proto__` property of the `Object` constructor function points to `null`

So why is this mechanism so powerful? Let's see some code
We define a constructor function and create 2 object using it

```
function MyPseudoClass(welcomeString){
    this.welcomeString = welcomeString;
}

var myPseudoClassObject1 = new MyPseudoClass("My precious");
var myPseudoClassObject2 = new MyPseudoClass("gollum gollum!");
console.log(myPseudoClassObject1);//Output MyPseudoClass { welcomeString: 'My precious' }
console.log(myPseudoClassObject2);//Output MyPseudoClass { welcomeString: 'gollum gollum!' }
``` 
So each of the objects created contains it's own copy of the property `welcomeString `. Let us assume we want the objects of `MyPseudoClass` to have the ability to print the `welcomeString` property. One way of doing it is adding a method to `MyPseudoClass`. Let's see how the objects would look then.

```
function MyPseudoClass(welcomeString){
    this.welcomeString = welcomeString;
	this.printWelcomeString = function(){
		console.log(this.welcomeString);
	};
}

var myPseudoClassObject1 = new MyPseudoClass("My precious");
var myPseudoClassObject2 = new MyPseudoClass("gollum gollum!");
myPseudoClassObject1.printWelcomeString();// Output My precious
myPseudoClassObject2.printWelcomeString();//Output gollum gollum!
console.log(myPseudoClassObject1);//Output MyPseudoClass { welcomeString: 'My precious', printWelcomeString: [Function] }
console.log(myPseudoClassObject2);//Output MyPseudoClass { welcomeString: 'gollum gollum!', printWelcomeString: [Function] }
``` 
So each object has its own copy of `printWelcomeString` method. In real life scenarios this will cause repetition of the method in all the object instances even though it's definition is the same for all the objects unlike the `welcomeString ` property which varies for each object. So this will lead to high memory consumption. Also any object inheriting from(i.e. pointing to the `prototype` property of the `MyPseudoClass` constructor function will not have access to the `printWelcomeString` method. How to avoid this? Simply move the `printWelcomeString` property from the constructor function to it's `prototype` object. Now we see the objects created don't have the method in them but still can access it as it's available to them in their prototype chain(i.e.  `myPseudoClassObject1.__proto__.printWelcomeString()`)

```
function MyPseudoClass(welcomeString){
    this.welcomeString = welcomeString;
}
var myPseudoClassObject1 = new MyPseudoClass("My precious");
var myPseudoClassObject2 = new MyPseudoClass("gollum gollum!");
MyPseudoClass.prototype.printWelcomeString = function(){
		console.log(this.welcomeString);
	};
myPseudoClassObject1.printWelcomeString();// Output My precious
myPseudoClassObject2.printWelcomeString();//Output gollum gollum!
console.log(myPseudoClassObject1);//Output MyPseudoClass { welcomeString: 'My precious'} 
console.log(myPseudoClassObject2);//Output MyPseudoClass { welcomeString: 'gollum gollum!' }
console.log(MyPseudoClass.prototype)//Output MyPseudoClass { printWelcomeString: [Function] }
``` 
Now let's create a child class and make it inherit the `MyPseudoClass`.

```
function MyPseudoClass(welcomeString){
    this.welcomeString = welcomeString;
}
console.log(MyPseudoClass.prototype); //Output MyPseudoClass {}
function MyPseudoChildClass(hobbitName){
	this.hobbitName = hobbitName;
}
MyPseudoChildClass.prototype.__proto__ = MyPseudoClass.prototype;
console.log(MyPseudoChildClass.prototype); //Output MyPseudoChildClass {}
console.log(MyPseudoChildClass.prototype.__proto__); //Output MyPseudoClass { printWelcomeString: [Function] }

var myPseudoChildClassObject = new MyPseudoChildClass("Frodo");
console.log(myPseudoChildClassObject) //Output MyPseudoChildClass { hobbitName: 'Frodo' }
console.log(myPseudoChildClassObject.__proto__) //Output MyPseudoChildClass {}
console.log(myPseudoChildClassObject.__proto__.__proto__)// Output MyPseudoClass { printWelcomeString: [Function] }
console.log(myPseudoChildClassObject.__proto__.__proto__.__proto__) // Output {}
console.log(myPseudoChildClassObject.__proto__.__proto__.__proto__.__proto__) //Output null
``` 
So, this proves what we just understood about the JavaScript prototype chain:

- `MyPseudoChildClass.prototype.__proto__ = MyPseudoClass.prototype;` This is where the actual inheritance happens and a link is added the existing prototype chain of **Object->MyPseudoClass**.


- `myPseudoChildClassObject.__proto__` points to the `prototype` property of `MyPseudoChildClass`.   `myPseudoChildClassObject.__proto__ === MyPseudoChildClass.prototype` is true

- `myPseudoChildClassObject.__proto__.__proto__` points to `portotype` property of `MyPseudoClass `. myPseudoChildClassObject.__proto__.__proto__ === MyPseudoClass.prototype` is true.

- `myPseudoChildClassObject.__proto__.__proto__.__proto__` points to the `prototype` property of `Object` constructor function. `myPseudoChildClassObject.__proto__.__proto__.__proto__ === Object.prototype` is true

- `myPseudoChildClassObject.__proto__.__proto__.__proto__ === Object.prototype` points to the `__proto__` property of prototype of the `Object` constructor function which is always `null`. `myPseudoChildClassObject.__proto__.__proto__.__proto__.__proto__===Object.prototype.__proto__` is true.

So this sums up the basics of how **prototype based inheritance** is achieved in JavaScript.

In the  [next post ](https://diganta.hashnode.dev/6-prototype-chain-objectcreate-and-more-ck5r5bqme06w4qks1xad55e27) we will learn about accessing the prototype chain, the remaining method of object creation `Object.create` and then compare the important difference between the various methods of object creation.

### References

 [https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes) 
 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#Using_prototypes_in_JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#Using_prototypes_in_JavaScript) 
 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) 
