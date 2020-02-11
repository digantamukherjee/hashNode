## 10. Lexical Environments, Execution Context, Scope and Hoisting in JavaScript

## Lexical Environment
This is an aspect of the ECMAScript specification that defines the relation or association of *Identifiers*(names) to specific variables and functions. 
Lexical environments exist in a nested fashion.
The Lexical environment is made up of 2 components:
1. Environment Record
2. A reference to an outer Lexical Environment (owing to the nested nature of Lexical environments). It is possible for this value to be `null`

There a some syntactic structures in JavaScript that cause a new Lexical environment to be created, viz. a *FunctionDeclaration*, a *WithStatement*, a *Catch* clause of a *TryStatement*. Now let's understand the constituents of a Lexical Environment in detail:
1. Environment Record: As the name suggests its a record that keeps track of all the identifiers that are created within the scope of the environment with which its associated.
2. Outer Environment reference: It is a pointer or reference to another Lexical environment that logically surrounds or encapsulates it. 

An outer lexical environment can also have its own outer environment.
 
One outer environment can serve as the outer environment for multiple inner lexical environments.

For example, the below function, `LazyCoder` can contain 2 other functions(`printFirstName` and `printLastName`). 
```
function LazyCoder(){
	var coderFirstName = "Harry";	
	var coderLastname = "Potter";
	var printFirstName = function(){
		return coderFirstName;
	}
	var printLastName = function(){
		return coderLastname;
	}
}``` 
Then the Lexical environments they create(as we noted earlier that *FunctionDeclaration* triggers creation of new Lexical environment) can be visualized as below:


![Lexical_environment.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1580145483186/lhE_v5JvA.png)

*Note: The Lexical environment is a purely abstract concept and the values cannot be accessed or manipulated in any way.*

Now let's see in details about Environment record

### Environment record
There are 2 kinds of environment records. 
1. Declarative environment records
2. Object environment records

Both of these types come with some features. For the purpose of our understanding we can treat Environment record as an abstract class with declarative and object environment record as its sub-classes. Now these abstract class have methods and these sub-classes have their own implementations for these. In addition to these common methods which each of these sub-classes have they have their own specific methods as well:

```
/*Note: this is just an analogy for understanding, 
how environment record and its types seem to look 
like abstract parent and child classes, and does not reflect 
actual implementation*/
//in this analogy * represent a generic data-type
abstract class EnvironmentRecord{
	abstract Boolean HasBinding(String N);
	abstract void    CreateMutableBinding(String N, Boolean D);
	abstract void    SetMutableBinding(String N,* V, Boolean S);
	abstract *       GetBindingValue(String N, Boolean S);
	abstract Boolean DeleteBinding(String N);
	abstract *       ImplicitThisValue()
}
class DeclarativeEnvironmentRecords extends EnvironmentRecord{
	//Common methods from EnvironmentRecord but implementation specific to DeclarativeEnvironmentRecords
	Boolean HasBinding(String N){ ... };
	void    CreateMutableBinding(String N, Boolean D){ ... }
	void    SetMutableBinding(String N,* V, Boolean S){ ... }
	*       GetBindingValue(String N, Boolean S){ ... }
	Boolean DeleteBinding(String N){ ... }
	*       ImplicitThisValue(){ ... }
	
	//Methods specific to DeclarativeEnvironmentRecords
	void CreateImmutableBinding(String N){ ... }
	void InitializeImmutableBinding(String N, * V){ ... }
}
class ObjectEnvironmentRecords extends EnvironmentRecord{
	//Common methods from EnvironmentRecord but implementation specific to ObjectEnvironmentRecords
	Boolean HasBinding(String N){ ... };
	void    CreateMutableBinding(String N, Boolean D){ ... }
	void    SetMutableBinding(String N,* V, Boolean S){ ... }
	*       GetBindingValue(String N, Boolean S){ ... }
	Boolean DeleteBinding(String N){ ... }
	*       ImplicitThisValue(){ ... }
	
	//Immutable bindings do not exist for ObjectEnvironmentRecords
}
``` 

 

The utility of each in creating and storing environment information for a Lexical environment can be defined as:

**Declarative environment records**:
These are used to define the syntactic behavior of *FunctionDeclarations*, *VariableDeclarations* and *Catch* clauses that directly associate *identifier* bindings with *values*. 

**Object environment records**:
These are used to defined behavior of *Program* and *WithStatement* which associate *identifier* bindings with *properties of some object* called as **binding object**. 

**The Global Environment**

This is an unique Lexical Environment. It is created before the code is executed.
Its environment record type is *Object environment record* whose binding object is the **global object**. Its outer environment's reference is `null` 

Now that we have an idea of what Lexical Environments are, lets understand **Execution Context**

## Execution Context
We have seen in a previous post that all the code in JavaScript can be classified into 3 categories: **Global code, Function code and Eval code.** And each of these there types of code leads to creation of different execution contexts. There can only be one Global Execution context inside which infinite number of function or eval execution contexts can reside in a nested fashion. The outer execution context cannot access the contents of the child execution context but the child can access the parent's execution context. And this is why **Global code** is part of a **Global execution context** which is the parent execution context of all code of JavaScript and is accessible from all parts of the JavaScript code. Let's visualize using code:

![executionContext.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1581442164537/OGLwriHU9.png)

Now if the function in the above example `MyAddition` is invoked the JavaScript interpreter creates a stack in the following order:


![executionContext.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1581454263708/JGxGtll07.gif)

As is evident from the above animation, the global execution context is created first and added to the stack, then `MyAddition`, then `getSum`, then `getA`. At this point `getA` is not creating a new execution context inside it as no other function or eval calls are present, so here code execution starts and once `getA` `return` is reached `getA` is popped out of the the stack, then `getB` execution context is created and similar to `getA`, `getB` code gets executed till `return` is reached and then `getB` gets popped out of the stack. This sequence of code execution and popping the execution context out of the stack goes on till only the global execution context is left in the stack at which point the program execution is complete.



```
//Global Execution Context(gec)
console.log("gec executed at: " + (new Date()).toISOString());
var myString = "Sum is: ";

function MyAddition(){//MyAddition Execution Context(maec)
    console.log("maec executed at: " + (new Date()).toISOString());
	var a = 10,
	    b = 20;
	
	function getA(){//getA Execution Context(gaec)
		console.log("gaec executed at: " + (new Date()).toISOString());
		return a;
	}
	
	function getB(){//getB Execution Context(gbec)
		console.log("gbec executed at: " + (new Date()).toISOString());
		return b;
	}
	
	function getSum(){//getSum Execution Context(gsec)
		console.log("gsec executed at: " + (new Date()).toISOString());
		return (getA() + getB());
	}
	console.log(myString + getSum());
}
/output
/*
gec executed at: 2020-02-11T18:06:46.816Z
maec executed at: 2020-02-11T18:06:46.818Z
gsec executed at: 2020-02-11T18:06:46.819Z
gaec executed at: 2020-02-11T18:06:46.819Z
gbec executed at: 2020-02-11T18:06:46.819Z
Sum is: 30
*/
``` 

The above execution is done by the interpreter in 2 stages:
### 1. **Creation Stage**

The is the stage when the *function* is called but before any code inside is gets executed. At this point interpreter gathers the following information:

- Scope chain is created
- Variables, functions and arguments are created
- **this** variable is determined

### 2. **Activation / Code Execution Stage**

Values of variables are assigned and reference to objects and function objects are resolved and the sequential execution of code happens.

So if we think of execution context as an object, at the creation stage the object looks like this:

```
var executionContextObject={
	"scopeChain" : {...},//contains variableObject + all parent execution context's variable objects
	"variableObject" : {/* function arguments, inner variables and function declarations */},
	"this" : {...}
}
``` 
So, in the below sample code the execution context during the creation and execution phases will look like:

```
function abc(m,n, b){
	var a = 1,
	    b = function(){
	};
	function c(){
	}
}
abc(5,6,4);
``` 
During creation phase:

```
abcExecutionContext = {
	scopeChain : {...},
	variableObject : {
		arguments : {
			m : 5,
			n : 6,
			b : 4,
			length : 3
		},
		a : undefined,
		b : undefined,
		c : pointer to function c()
	},
	this: {...}
}
``` 
During execution phase

```
abcExecutionContext = {
	scopeChain : {...},
	variableObject : {
		arguments : {
			m : 5,
			n : 6,
			b : 4,
			length : 3
		},
		a : 1,
		b : pointer to anonymous/unnamed function,
		c : pointer to function c()
	},
	this: {...}
}
``` 
*The creation stage happens even before the execution of the first line of function's code. This means when the first line of code is executed the interpreter will be having the information of all the variables and functions present inside of `abc`. Also variables will only be in declared state whereas function names will be pointing to the function object in memory.* **This phenomenon is called as Hoisting and this is very important to realize why and how JavaScript code behaves the way it does**


During this hoisting phase for each variable name, declaration happens only once and the next declaration in the same execution context is ignored. 

Now in the execution phase as and when assignment statements are executed the variables start getting populated.

It is important to note at this point that the **Variable object** we have been talking about it an ECMA-262-3(3rd edition of the specification) whereas since the 5.1 edition this is referred to as **Lexical environment** which we have discussed earlier in this post.

Now that we have some idea about how JavaScript interpreter behaves under the hood it will be possible to write code with better understanding of its outcome. 

In the next post we shall discuss about closures.

**References**:

 [http://ecma-international.org/ecma-262/5.1/#sec-10.2.1.1](http://ecma-international.org/ecma-262/5.1/#sec-10.2.1.1) 

 [http://ecma-international.org/ecma-262/5.1/#sec-10.2.1.2](http://ecma-international.org/ecma-262/5.1/#sec-10.2.1.2) 

