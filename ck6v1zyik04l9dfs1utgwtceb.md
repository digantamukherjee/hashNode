## 13. Operators in JavaScript

Now that we have some idea about JavaScript closures, before proceeding further we need to have some idea about **Operators** in JavaScript.  
  
So what are the *Operators* that JavaScript support and how are they prioritized?

# Operators  
An operator is a programming construct that does an action on one or more operand to produce the corresponding result.  
In JavaScript, as in other programming languages, operators can be classified in the following ways:
Based on the **number of operands** that the operator works upon, operators can be classified as:  
- **Unary** This operates on 1 operand  
   Example `a++`, `++a`, `!a`, `+a`
- **Binary** This operates on 2 operand  
   Example `a+b`, `a-b`, `a*b`, `a/b`
- **Ternary**This operates on 3 operand  
   Example `a ? b : c`  
  
Based on their purpose on the language operators are classified as:  
- **Assignment Operator**:  
   The basic assignment operator `=` takes the value of the RHS expression and assigns it to the LHS operand. This operator can also be written in its compound form:  
   `<operand1> <operator>= <operand2>`  
   and this translates to the below code:  
   `<operand1> = <operand1> <operator> <operand2>`  
   Example:  `a %= b` translates to `a = a % b`  
   Assignment operator(`=`) can be compounded with Addition(`+`), Subtraction(`-`), Multiplication(`*`), Division(`/`), Remainder(`%`), Exponentiation(`**`), Left Shift(`<<`), Right Shift(`>>`), Unsigned Right Shift(`>>>`), Bitwise AND(`&`), Bitwise OR(`|`) and Bitwise XOR(`^`).

There is one more construct introduced into JavaScript for ease of assignment and its called as **Destructuring Assignment**.  
The **Destructuring Assignment** makes it possible to unpack values from arrays or properties of objects into distinct variables.  
Example:  
```
var a,b,rest;

[a,b] = [10,20] //assigning values from an array literal to individual variables
console.log(a);//10
console.log(b);//20

var arr1 = [30,40,50,60,70];
[a,b] = arr1;//assigning values from an array object to individual variables
console.log(a);//30
console.log(b);//40

[...restOfTheData] = arr1;//A special parameter which captures all of the values of the arr1
console.log(restOfTheData);

[a,b,...restOfTheData] = arr1;
console.log(a);//30
console.log(b);//40
console.log(restOfTheData);//[ 50, 60, 70 ]//see how the rest parameter capture the remaining part of the array
``` 
What if we change the property names to something else?  
```
({a,b} = {d:12, e: 13});
console.log(a);//undefined
console.log(b);//undefined
```
We can also assign default values to variable in case the value unpacked from the array is `undefined`. Its a safety check to avoid a value of `undefined` being set to a variable:  
```
[a=99,b=100] = [2];
console.log(a);//2
console.log(b);//100
```
**Swapping** of variables becomes easy using this construct:  
```
[a,b] = [b,a]
console.log(a);//100
console.log(b);//2
```
Multiple array elements can be swapped easily using this:  
```
var arr2 = [1,2,3];
[arr2[0], arr2[1], arr2[2]] = [arr2[2], arr2[0], arr2[1]];
console.log(arr2);//[ 3, 1, 2 ]
```
**Object properties **can be destructured as follows:  
```
({a,b} = {a:12, b: 13});//
console.log(a);//12
console.log(b);//13
```  
Notice the usage of () to enclose destructuring while using objects. This is because `{a,b} = {a:12, b: 13}` is not a valid syntax as `{a,b}` on the LHS is considered a block and not object literal. Also `const {a,b} = {a:12, b: 13}` is valid.  
A property from an object can be unpacked and assigned to a new variable name than the object property:  
```
var obj1 = {x:2,y:3}
const {x:abc, y:def} = obj1;
console.log(abc);//2
console.log(def);//3
```
Default value can also be assigned similar to arrays to avoid `undefined` being set to a variable:  
```
const {p = 3, q = 7} = {p:77};
console.log(p);//77
console.log(q);//7

const {p:pp=99, q:qq=100} = {p:55};
console.log(pp);//5
console.log(qq);//100
```
This kind of destructuring assignment is valid in all scenarios where an object assignment can happen, like *formal parameters of a function*, *array and objects existing in a nested fashion*, or *even a combination of array and objects present in a nested fashion*.  
```
//combination of array and objects present in a nested fashion
var objArray = [
    {firstName: "John", lastName: "Doe"},
	{firstName: "Jane", lastName: "Doe"}
];
const [{firstName:name1}] = objArray;
const [,{firstName:name2}] = objArray;
console.log(name1);
console.log(name2);

//formal parameters of a function
function ArithmeticOperation({operation = "Addition", oerpands = {a:0, b:0}}){
	switch(operation){
		case "Addition":
			return oerpands.a+oerpands.b;
			break;
		default:
			return "Not an arithmetic operation";
			break;
	}
}
console.log("Arithmetic result is: " + ArithmeticOperation(arithmeticObjectAddition));
```
Also, destructuring assignment looks up the prototype chain for references:  
```
var objA = {m:2,n:3};
objA.__proto__.t = 4;
const {m, n, t} = objA;
console.log(m);//2
console.log(n);//3
console.log(t);//4
```
Enough about destructuring, let's move on to the next type of operator.  
  
- **Comparison Operator**:  
It returns a boolean value based on the result of comparison.  
The types that can be compared are numerical, string, boolean or objects.  
Strings are compared btyewise using their *Unicode* values.  
For objects their memory references they are pointing to are compared.  
If different types of operands are compared, JavaScript will try to convert the values before comparison(usually converted to numeric).  
Equality Comparison can be *non-strict* or *strict*.  
*Strict* comparison does not try to convert operands to the same type.  
The various comparison operators are Equal(`==`), Strict Equal(`===`), Not Equal(`!=`), Strict Not Equal(`!==`), Greater Than(`>`), Greater Than Or Equal(`>=`), Less Than(`<`), Less Than or Equal(`<=`). An example of object comparison:  
```
var o1 = {
	a:1
};
var o2 = o1;
console.log(o1 === o2);//true
o2 = {
	a:1
};
console.log(o1 === o2);//false
```
In the above example, when first the `o2 = o1` statement is executed, both `o1` and `o2` are referring to the same object. However once the object `o2` gets redefined, even though the new object it is pointing to has the exact same properties as `o1`, it is not the same object, because the object literal used while redefining `o2` is at a different memory location than the previous object literal.  
  
- **Arithmetic Operators**:  
These include the standard Addition(`+`), Subtraction(`-`), Multiplication(`*`), Division(`/`). (This is interesting: `0.1+0.2==0.3` ?)
Along with these JavaScript gives us the below operators:  
   **Remainder(`%`)** - Binary operator that returns the integer remainder of a division operation.  
   **Increment(`++`)** - Unary operator that can be used as both postfix and prefix fashion. It increments the operand's value by 1and returns it in case of prefix and return the operand and then increments by 1 for postfix.  
   **Decrement(`--`)** - Same as the Increment operator, only it decrements operand by 1.  
   **Unary Negation(`-`)** - Returns negation of its operand.  
   **Unary plus(`+`)** - Tries to convert operand to number.  
   **Exponentiation(`**`)** - Calculates base to exponent.  
- **Bitwise Operators**:  
This convers the operator to its corresponding 32 bit and then operates on it. Its of following types:  
   **Bitwise AND(`&`)** -Returns 1 at the position where the corresponding bits of both operands are 1.  Example: `1&2` evaluates to `01&10 = 0` 
   **Bitwise OR(`|`)** -Returns 0 at the position where the corresponding bits of both operands are 0.  Example: `1|2` evaluates to `01|10 = 3` 
   **Bitwise XOR(`^`)** -Returns 0 at the position where the corresponding bits of both operands are **same** and 1 where the corresponding bit are different.  Example: `1^2` evaluates to `01^10 = 3`   
   **Bitwise Not(`~`)** -Inverts the bits of its operand. This operation on any number `x` yields `-(x+1)`. Example: `~1` equals to `-(1+1)` which is `-2`
   **Left Shift(`<<`)** - For this operation `x << y` it shifts `x` by `y` bits to the left and adds `y` zeros to the right. For example: ``2<<1` in 32 bit is `(10)<<1` which becomes `(100`) which is `4`.  
   **Right Shift(`>>`)** - For this operation `x >> y` it shifts `x` by `y` bits to the right discarding the bits shifted off. For example: `2>>1` in 32 bit is `(10)>>1` which becomes `(1)`) which is `1`.  
  
- **Logical Operators**:  
The return boolean values if operated upon boolean values and might return non-boolean values if operated on non-boolean values.  
The can be **Logical AND(`&&`), Logical OR(`||`) and Logical NOT(`!`).**  
Examples:  

```
//logical operators
console.log(false && false);//false
console.log(false && true);//false
console.log(true && false);//false
console.log(true && true);//true

console.log(false || false);//false
console.log(false || true);//true
console.log(true || false);//true
console.log(true || true);//true

//short-circuit operation
//&& returns the last operation if both operands are truthy
console.log(2 && 3 && 9);//9
//&& returns first truthy operand if exists else false
console.log(2 || 3 || 5);//2
console.log(0 || 1 || 2);//1 - since 0 is falsy value in 
avascript

//very common but error prone way of setting default values
var a = 0;
var b = a || 5;//Here 0 might be a valid value for our scenario, yet it gets replaced by 5 because of short-circuit evaluation
console.log(b);
``` 
In the above code to avoid the error prone way of setting default values we can make use of  [Nullish coalescing operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Assignment).  
  
- **String Operators**:  
The `+` operator can be used to concatenate 2 strings.  
  
- **Conditional ternary Operator**:  
This evaluates the statement `condition ? val1 : val2` , if `condition` is true return `val1` else `val2`.  
  
- **Comma Operator**:   
The comma operator evaluates both of the operands and returns the value of the last operand. Common usage is in `for` loop as   
`for (var i = 0, j = 2; i <= j; i++, j--)`  
Example:

```
var a,b,c;
a = b = 2, c = 7;//without comma value of "b" is assigned to "a"
console.log(a);//2
a = (b = 2, c = 7);//with comma, the last expression is evaluated and its value is returned
console.log(a);//7
``` 
  
- **Unary Operator**:   
Following are the operators:  
   **`delete` Operator**:  
It can delete an object, its properties or an element of a specific index of an array. If operation is successful it returns `true` else `false`.  
In case of deleting array element it is not the same as splice or pop, as the length of the array is not affected. Only the element at the position gets a value of `undefined`.  
  
   **`typeof` Operator**:   
It returns the type of the operand.  
  
   **`void` Operator**:   
It evaluates the given expression and then returns `undefined`.  
`void` keyword can cause a function to be treated as a function expression and hence can be used to invoke IIFE(Immediately Invoked Function Expression).  

Now that we have a fair bit of idea about the operators JavaScript support we need can see the precedence order or how they are prioritized:  
**Associativity** : This determines how operators of same precedence are evaluated. For the below expression:  
` a OPERATOR b OPERATOR c`  
Left-associativity (left-to-right) means that it is processed as (a OPERATOR b) OPERATOR c, while right-associativity (right-to-left) means it is interpreted as a OPERATOR (b OPERATOR c).  

```
3 > 2 > 1 // this returns false as "3 > 2" returns "true", then "true" is converted to 1 and "1 > 1" is false.
```   
The precedence of operators in decreasing order is as follows:  
a) Precedence(21) - Grouping  
b) Precedence(20) - Member Access, Computed Member Access, new (with argument list), Function Call, Optional chaining  
c) Precedence(19) - new (without argument list)  
e) Precedence(18) - Postfix Increment, Postfix Decrement  
f) Precedence(17) - Logical NOT, Bitwise NOT, Unary Plus, Unary Negation, Prefix Increment, Prefix Decrement, typeof, void, delete, await  
g) Precedence(16) - Exponentiation  
h) Precedence(15) - Multiplication, Division, Remainder  
i) Precedence(14) - Addition, Subtraction  
j) Precedence(13) - Bitwise Left Shift, Bitwise Right Shift, Bitwise Unsigned Right Shift  
k) Precedence(12) - Less Than, Less Than Or Equal, Greater Than, Greater Than Or Equal, in, instanceof  
l) Precedence(11) - Equality, Inequality, Strict Equality, Strict Inequality  
m) Precedence(10) - Bitwise AND  
n) Precedence(9) - Bitwise XOR  
o) Precedence(8) - Bitwise OR    
p) Precedence(7) - Nullish coalescing operator  
q) Precedence(6) - Logical AND  
r) Precedence(5) - Logical OR  
s) Precedence(4) - Conditional  
t) Precedence(3) - Assignment  
u) Precedence(2) - yield, yield*
v) Precedence(1) - Comma / Sequence  
  
Well that sums up the operators present in JavaScript. In the next post we shall move on to JavaScript Built-in Objects.

**References**  
 [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)  
 [Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Assignment)  
 [Operator Precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) 