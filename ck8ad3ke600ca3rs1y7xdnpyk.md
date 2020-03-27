## 19. Built-in Objects of JavaScript/ECMAScript -Part IV

In the  [previous post](https://diganta.hashnode.dev/16-built-in-objects-of-javascriptecmascript-part-iii-ck891r4rs00719as1ft5r7g03) we have explored the very significant built-in **String** object. Now we shall see few more built-in objects.  
  
# Boolean Objects  
## Boolean Constructor called as a Function  
When it is called as a function then it performs a type conversion on the value provided and return a Boolean value and not a Boolean Object. 
```
var b = new Boolean("somethign");
console.log(b);
/*
Boolean{
	__proto__: Boolean
	[[PrimitiveValue]]: true
}
*/
```  

## The Boolean Constructor  
When it is invoked with the `new` expression it is a constructor call and it initializes the newly created object.  
The newly created Boolean object has the following properties:  
- `[[Prototype]]` - It is set to `Boolean.prototype` by default.  
- `[[Class]]` - It is set to `Boolean` by default.  
- `[[PrimitiveValue]]` - This is the value that is obtained by calling the Boolean Constructor as a Function over the *value* supplied, which does a type conversion to a Boolean literal.  
- `[[Extensible]]` - This is set to `true` by default.  
  
### Properties of the Boolean Constructor  
#### Boolean.Prototype  
The initial value of this property is the **Boolean prototype object**. The attributes of *Boolean.Prototype* are `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`.  
##### Properties of the Boolean Prototype Object  
This is a Boolean object whose value is `false` and its built-in `[[Prototype]]` property is the built-in Object prototype object.
- **Boolean.prototype.constructor**: Its value is the built-in Boolean constructor.  
- **Boolean.prototype.toString()**: This return either "true" or "false" if the object is of type Boolean else throws a `TypeError` exception.  
- **Boolean.prototype.valueOf()**: Return the value of the object if its of type Boolean else throws a `TypeError` exception.  
  ```
var b = new Boolean("somethign");
console.log(b);//[Boolean: true]
```
  
### Properties of Boolean Instances  
They have 2 properties: `[[Class]]` internal object which is of value **Boolean** and `[[PrimitiveValue]]` which is the Boolean value represented by the object.  

# Number Objects  
## The Number constructor called as function  
When the number constructor is called as a function, it does a type conversion of the value passed to it to a number. If a value was supplied it return the converted value else returns +0.  
```
console.log(Number(8));//8
console.log(Number("8"));//8
console.log(Number(""));//0
console.log(Number());//0
```
## Number Constructor  
It initializes the number object.  
```
var a = new Number(9);
console.log(a);
/*
Number{
	__proto__: Number
	[[PrimitiveValue]]: 9

}
*/
```
The constructor does type conversion if non-numeric values are provided. If its a string that contains non-numeric characters the value set to the object is `NaN`, if its Boolean then the value is 1 for `true` and 0 for `false`. If the a string is provided containing on numeric characters(both positive and negative) then its converted to its corresponding value.  
- `[[Prototype]]` - It is set to `Number.prototype` by default.  
- `[[Class]]` - It is set to `Number` by default.  
- `[[PrimitiveValue]]` - This is the value that is obtained by calling the Number Constructor as a Function over the *value* supplied, which does a type conversion to a Number literal. If nothing is supplied it set +0.  
- `[[Extensible]]` - This is set to `true` by default.  
  
### Properties of Number Constructor  
#### Number.prototype  
its value is the number prototype object. Its attributes are `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`. It has the following properties which are like constant values.  
  - **Number.MAX_VALUE**: This is the largest positive finite value of Number type. It is approximately 1.7976931348623157 × 10308.  
  - **Number.MIN_VALUE**: This is the smallest positive value of the Number type. Its value is 5 × 10^‑324.     
  - **Number.NaN**: This is the system defined value of `NaN` and is not mutable.  
  - **Number.NEGATIVE_INFINITY**: Its value is −∞ and it is not mutable.  
  - **Number.POSITIVE_INFINITY**: It value is ∞ and it is not mutable.  
   
#### Properties of Number prototype Object  
It is a Number object whose value is +0.  It s`[[Class]]` is **"Number"** and `[[Prototype]]` points to built in **Object** prototype object.
  - **Number.prototype.constructor**: This is the built-in number constructor.  
  - **Number.prototype.toString([radix])**: This is not a generic method unlike other implementations of `toString()` because this method comes with a parameter which is the `radix` value. If this implementation is not invoked on a `Number` value a `TypeError` exception is thrown.  
`Radix` is an *integer optional parameter* which ranges from **2 to 36**. If radix is not present or is undefined then the default value of 10 is assigned to it. If the radix does not evaluate to a number between 2 and 36(including) then a `RangeError` exception is thrown. With the given radix it converts the number value to string and returns it.  
  - **Number.prototype.toLocalString()**: This also returns a string but as per the host environment's current locale.  
  - **Number.prototype.valueOf()**: This will return the number value. This is specific to Number and throws a `TypeError` exception if used on any other kind of object.  
  - **Number.prototype.toFixed(fractionDigits)**: This returns a string value containing the number is decimal format with *fractionDigits* digits after the decimal point. Default value of *fractionDigits* is 0.  
  - **Number.prototype.toExponential(fractionDigits)**: Returns a string with number value converted to decimal exponential notation with 1 digit before the decimal and *fractionDigits* digits after the decimal point. If *fractionDigits* is undefined then the significand value is increased to represent the number as uniquely as possible.  
  - **Number.prototype.toPrecision(precision)**: This is same as above except that after the decimal point here will be *precision-1* digits. If *precision* is undefined then the `toString()* method is applied.  
  
#### Properties of Number instances  
They have 2 internal properties: `[[Class]]` whose value is **Number** and `[[PrimitiveValue]]` which is the value represented by the number object.  
  
# Math Object  
The math object is an helper object functionality in JavaScript which is used for different computations. Its `[[Prototype]]` points to the Object prototype object and `[[Class]]` has an internal property of **Math**.  
It **does not** have a `[[Construct]]` internal property, i.e. it lacks a construct and the `new` keyword cannot be used on it. It does not have a `[[Call]]` property either and hence cannot be invoked as a function. It however has some *Value properties* and some *Function properties* defined on it, similar to methods of Static classes.  
## Value properties of the Math object  
  - **E**: This is a number value for **e** which is the base of natural logarithms. Its value is approximately equal to `2.7182818284590452354`.  
  - **LN10**: This is the number value for the natural logarithm of 10 which is approximately equal to `2.302585092994046`.  
  - **LN2**: This is the number value for natural logarithm of 2 which is approximately equal to `0.6931471805599453`.  
  - **LOG2E**: This is the number value of base-2 logarithm of **e** and its approximate value is `0.4342944819032518`.  
  - **PI**: This is the number value representing π whose approximate value is `3.1415926535897932`.  
  - **SQRT1_2**: This is the number value for square root of `½` whose approximate value is `0.7071067811865476`.  
  - **SQRT2**: This is the number value for square root of 2 and its approximate value is `1.4142135623730951`.  
All of these above values are immutable: `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`
  
## Function Properties of Math Object  
All arguments of these functions are implicitly converted to numbers.  
- **abs(x)**: returns the **absolute value** of `x`. Return values for special cases like `NaN`, `-0` and `-∞` are `NaN`, `+0` and `+∞` respectively.  
- **acos(x)**: returns the **arc cosine** of `x`. Result is in **radians** and range is `+0 to +π` for all `0<= x < 1`. If `x=1`, result is `+0`, and for all other cases it is `NaN`.  
- **asin(x)**: returns the **arc sine** of `x`. Result is in **radians** and ranges from `−π/2 to +π/2` for all `0 < x < 1`. If `x=+0` result is +0, if `x=-0` result is -0. For all other values result is `NaN`.  
- atan(x)**: returns the **arc tangent** of `x`. Result is in **radians** and ranges from `−π/2 to +π/2`. For `NaN`, +0 and -0 the results are `NaN`, +0 and -0 respectively. For +∞ and -∞ the values are approximation of +π/2 and -π/2 respectively.  
- **atan2(y,x)**: It returns the **arc tangent** of the quotient y/x where the signs of arguments are used to determine the quadrant of result. The result is expressed in **radians** from `−π to +π`.  
- **ceil(x)**: Returns the next possible integer value of `x`. The value of `Math.ceil(x)` is the same as the value of `-Math.floor(-x)`.  
- **cos(x)**: Returns the **cosine** of `x` in **radians**.  The argument is expressed in **radians**
- **exp(x)**: returns `e` raised to the power of `x`.  
- **floor(x)**: If x is not an integer value return the immediately less than `x`.  
- **log(x)**: returns the natural logarithm of `x`.  
- **max ( [ value1 [ , value2 [ , … ] ] ] )**: returns the largest argument provided. If not arguments are given the result is -∞, for `NaN` it is `NaN`. +0 is greater than -0.  
- **min ( [ value1 [ , value2 [ , … ] ] ] )**: returns the smallest argument provided. All other conditions are same as `max(x)`.  
- **pow(x,y)**: it returns the value of `x` raised to the power `y`.  
- **random()**: returns a positive number in-between 0 and 1.  
- **round(x)**: returns the integer closed to `x`. A in case of equidistant condition, closeness to +∞ is calculated. The result of this operation on `NaN`, -0, +0, +∞, -∞ is NaN`, -0, +0, +∞, -∞ respectively.  
For **0<x<0.5** result is **+0**. For  **-0.5<x<0** result is **-0**.  
- **sin(x)**: returns the approximate value of **sine** of `x`. The argument is expressed in **radians**.  
- **tan(x)**: returns the approximate value of **tangent** of `x`. The argument is expressed in radians.  
- **sqrt(x)**: return the square root of `x`. When value of `x<0` or `x=NaN` the value is `NaN`. For +0, -0 and +∞ the value is +0, -0 and +∞ respectively.  
  
So these were 3 more built-in objects that are used a lot. In the next post we will know about the remaining few.  
  
  
###### Reference:  
 [https://www.ecma-international.org/ecma-262/5.1/](https://www.ecma-international.org/ecma-262/5.1/) 