## 12. Closures in JavaScript

In our  [last post](https://diganta.hashnode.dev/11-this-and-call-bind-and-apply-in-javascript-ck6jr64ob00y5dfs1cftq7u8e)  we understood the execution context completely by understanding the `this` variable and the ways it gets its value and how we can set its value explicitly.

At this point, we are equipped enough to dive into the feature of **closures**.

# Closures

> A Closure is said to be created when an inner function which is inside an 
 outer function is made available outside the outer function and its able to access the execution context of the outer function.

Let's see an example:
**Example 1**:

```
function outer(outerParam){
	var outerVar = "hello";
	return function inner(innerParam){
		console.log(outerParam + " " + outerVar + " " + innerParam);
	}
}
var globalVar = outer("say");
globalVar("to all");//say hello to all
``` 
Here the outer function is `outer` and its execution context has the variable `outerParam` and `outerVar`.

The inner function is `inner` and its execution context has the variable `innerParam`.

The statement `var globalVar = outer("say");` pulls out the `inner` function along with the reference to the execution context of it's parent function `inner` and assigns it the variable `globalVar`. Due to this when `globalVar` is invoked it is actually a copy of the `inner` function, it is able to access it's parent's execution context even though parent function's execution has ended and ideally variables in the parent function's execution context should have been garbage collected. This phenomenon is called as *Closure*

**Example 2**:

```
var x = function(){
	var y = 10;
	return function(g){
		return y + g;
	}
}

var z = x();
x= null;
console.log(z(9));//19
``` 
Here we see even though we explicitly remove the reference to the anonymous function which was earlier referenced by the variable `x`, the closure function referenced by `z` is still able to access it.

**Example 3**:

Closure are useful to make Function factory. For example if we wish to create an addition factory where we want to generate a function which would return the sum of a constant number and the supplied number.

```
function AdditionFactory(num){
	return function(a){
		return num + a;
	}
}

var add2 = AdditionFactory(2);
console.log(add2(2));//4

var add3 = AdditionFactory(3);
console.log(add3(2));//5
``` 
**Example 4**:

```
function closureReferenceOuter(){
	var varToRefer = 100;
	return	{//here an object is returned which contains the closure functions
				setVarToRefer: function(newVar){
					varToRefer = newVar;
				},
				getVarToRefer: function(){
					return varToRefer; 
				}
			}
}
var tempClosure = closureReferenceOuter();
console.log(tempClosure.getVarToRefer());//100
tempClosure.setVarToRefer(101);
console.log(tempClosure.getVarToRefer());//101
``` 
Closure **store references to outer function's variable and the actual value is not stored.** If the outer function's variables get modified in some way, the closure function will also reflect it

**Example 5**:
Closures can be used to emulate private variable in JavaScript.

```
function MyClass(){
	var counter = 0;
	return {
		incrementCounter : function(){
			++counter;
		},
		decrementCounter : function(){
			--counter;
		},
		getCounter : function(){
			return counter;
		}
	};
}

var newCounter = MyClass();
console.log(newCounter.getCounter());//0
newCounter.incrementCounter();
newCounter.incrementCounter();
newCounter.incrementCounter();
console.log(newCounter.getCounter());//3
newCounter.decrementCounter();
console.log(newCounter.getCounter());//2
``` 
In the above example there is no way to directly access the `counter` variable which makes it a private variable.

**Example 6**:

Let us see one more very common example where the usage of closure, seem to cause inconsistencies in the code.


```
function buildFunctions(){
	var arr = [];
	for(var  i = 0; i < 3; i++){
		arr.push(
			function(){
				console.log(i);
			}
		);
	}
	return arr;
}
var fs = buildFunctions();
fs[0]();//3
fs[1]();//3
fs[2]();//3
``` 
Ideally we expect this to print 0, 1 and 2 but due to closure the reference to "i" in the functions pushed to the array does not end even after the function gets added to the array in the Nth position and every time i++ is executed the value in the functions already pushed to the array also gets updated.

So to solve this we need to constrict or restrict the scope of the variable `i`. Here the new JavaScript ES6 programming construct called `let` can be used to achieve the desired result:

> The `let` statement declares a block scope local variable, optionally initializing it to a value.

```
function buildFunctionsSafe(){
	var arr = [];
	for(let i = 0; i<3; i++){
		arr.push(
			function(){
				console.log(i);
			}
		);
	}
	return arr;
}

var fss = buildFunctionsSafe();
fss[0]();//0
fss[1]();//1
fss[2]();//2
``` 

Or another way of rectifying the code is to use **Immediately Invoked Function Expression** in conjugation with closure:

```
function buildFunctionsSafeClosure(){
	var arr = [];
	for(var i = 0; i<3; i++){
		arr.push(
			(function(j){
				return function(){
					console.log("correct " + j);
				}
			}(i))
		);
	}
	return arr;
}

var fssc = buildFunctionsSafeClosure();
fssc[0]();//correct 0
fssc[1]();//correct 1
fssc[2]();//correct 2
``` 

> Callback functions in JavaScript are also closures

Callback functions derived from programming paradigm called FUNCTIONAL programming which specifies use of functions as arguments.
Callback functions are closures - As the callback function is passed as an argument to another function, it will be executed at some point inside the containing function's body as if the callback function was defined in the containing function body. This is how callbacks behave as closures and have access to the containing function's variables, parameters and also the global scope

Event handlers for DOM elements are also closures.

So now we have a fair idea of what are closures, how and why they behave the way they do.

In the next post we will look in details about JavaScript operators.

**References**

 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) 