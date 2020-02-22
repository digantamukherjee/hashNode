## 15. Built-in Objects of JavaScript/ECMAScript -Part II

In our  [last post](https://diganta.hashnode.dev/14-built-in-objects-of-javascriptecmascript-part-i-ck6v7d8sw04mrdfs1sh5a0gj0)  we had started learning in-depth about the build-in objects of JavaScript and we learned about Global and Object objects. Now let's move ahead and see a few more:

# Function Objects #  
A function can be called as both *Function* and as *Function Constructor*. In both of the cases it creates and initializes a new Function object.   
A prototype property is automatically created for every function, to provide for the possibility that the function will be used as a constructor.  
### Properties of the Function Constructor ###  
**a) Function.prototype** - The initial value of Function.prototype is the standard built-in Function prototype object .  
This property has the attributes `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`.  
**b) Function.length** - A data property with value 1. Its attributes are `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`  
### Properties of the Function Prototype Object ###  
The Function Prototype Object is a Function object. The length property of the Function prototype object is 0.  
**a) Function.prototype.constructor**:The initial value of Function.prototype.constructor is the built-in Function constructor.  
**b)Function.prototype.toString ( )**:The toString() method returns a string representing the source code of the function.  
**c) Function.prototype.apply (thisArg, argArray)**: This is to call a function and pass the `this` object and pass an array of parameters.  
**d)Function.prototype.call (thisArg [ , arg1 [ , arg2, … ] ] )**: This is to call a function and pass the `this` object and comma separated parameters.  
**e)Function.prototype.bind (thisArg [, arg1 [, arg2, …]])**: This binds the `this` object to the function and then returns the function.  
**f) [[Call]]**: Call property of the function. Makes the function callable.  
**g) [[Construct]]**: Added to function be default to make it a function constructor.  
**h) [[HasInstance]] (V)**: Check whether the prototype of this function is equal to any prototype in the prototypial chain of V. So any Function instance is also an instance of Object, since Function inherits from Object.  
  
### Properties of Function Instances###
**a) length**: The value of the length property is an integer that indicates the “typical” number of arguments expected by the function.  
**b) prototype**: The value of the prototype property is used to initialise the [[Prototype]] internal property of a newly created object before the Function object is invoked as a constructor for that newly created object. This property has the attribute { [[Writable]]: true, [[Enumerable]]: false, [[Configurable]]: false }. Function objects created using Function.prototype.bind do not have a prototype property.  
**c) [[HasInstance]] (V)**: Check whether the prototype of this function is equal to any prototype in the prototypial chain of V. So any Function instance is also an instance of Object, since Function inherits from Object.  
**d) [[Get]] (P)**: It is an internal method used for getting the property **P**.  
  
# Array Objects #  
This can be called as both a function and as a constructor(with the `new` keyword). Both yields the same result.   

```
var b = new Array();
console.log(b.length);//0
b.push(1);
console.log(b.length);//1

var c = Array();
console.log(c.length);//0
c.push(2);
console.log(c.length);//1
``` 
### Properties of the Array Constructor ###  
**a) Array.prototype**: The initial value of Array.prototype is the Array prototype object. This property has the attributes `{ [[Writable]]: false, [[Enumerable]]: false, [[Configurable]]: false }`.  
**b) Array.isArray ( arg )**: The isArray function takes one argument arg, and returns the Boolean value true if the argument is an object whose class internal property is "Array"; otherwise it returns false.   
### Properties of the Array Prototype Object ###  
**a) Array.prototype.constructor**: The initial value of Array.prototype.constructor is the standard built-in Array constructor.  
**b) Array.prototype.toString ( )**: Returns the array in stringified form. The toString function is intentionally generic; it does not require that its this value be an Array object. Therefore it can be transferred to other kinds of objects for use as a method. Whether the toString function can be applied successfully to a host object is implementation-dependent.  
**c) Array.prototype.toLocaleString ( )**: The result of calling this function is intended to be analogous to the result of toString, except that the result of this function is intended to be locale-specific.  
**d) Array.prototype.concat ( [ item1 [ , item2 [ , … ] ] ] )**:  This merges 2 arrays and returns the resulting array. Original arrays are not modified.  
**e) Array.prototype.join (separator)**: The elements of the array are converted to Strings, and these Strings are then concatenated, separated by occurrences of the separator. If no separator is provided, a single comma is used as the separator.  
**f) Array.prototype.pop ( )**: The last element of the array is removed from the array and returned.  
**g) Array.prototype.push ( [ item1 [ , item2 [ , … ] ] ] )** : The arguments are appended to the end of the array, in the order in which they appear. The new length of the array is returned as the result of the call.  
**h) Array.prototype.reverse ( )**: The elements of the array are rearranged so as to reverse their order. The object is returned as the result of the call.  
**i) Array.prototype.shift ( )**: The first element of the array is removed from the array and returned. Similar to pop(), but the first element instead of last gets removed.  
**j) Array.prototype.slice (start, end)**: The slice method takes two arguments, start and end, and returns an array containing the elements of the array from element start up to, but not including, element end (or through the end of the array if end is undefined). If start is negative, it is treated as length+start where length is the length of the array. If end is negative, it is treated as length+end where length is the length of the array.    
```
var arr = [1,2];
console.log(Array.isArray(arr));//true

console.log(arr.toString());//1,2

console.log(arr.toLocaleString ());//1,2

var arr2 = arr.concat([3,4]);
console.log(arr2);//[ 1, 2, 3, 4 ]

arr.push(5);
console.log(arr);//[ 1, 2, 5 ]

console.log(arr.pop());//5
console.log(arr);//[ 1, 2 ]

arr.reverse();
console.log(arr);//[ 2, 1 ]

console.log(arr.shift());//2
console.log(arr);//[ 1 ]

console.log(arr2.slice(1,3));//[ 2, 3 ]
console.log(arr2);//[ 1, 2, 3, 4 ] slice does not change the original array
console.log(arr2.slice(3));//[ 4 ]
console.log(arr2.slice(-1));//[ 4 ] === length+start = 4+(-1) = 3 === arr2.slice(3)
console.log(arr2.slice(-8));// === arr2.slice(max(4+(-8),0)) === arr2.slice(0)
``` 
**k) Array.prototype.sort (comparefn)**: Sort the array based on the logic defined by the compare function. The compare function accepts two arguments x and y and returns a negative value if x < y, zero if x = y, or a positive value if x > y.  
Things to note here are: non-existent property values always compare greater than undefined property values and undefined always compares greater than any other value. So the order is:
> all other values < undefined values < non-existent values  

```
var origArr = [2,undefined,,23,7,55,7,10];//array with one empty item and one value undefined.
origArr.sort(function(x,y){
	return x < y;
});
console.log(origArr);//[ 55, 23, 10, 7, 7, 2, undefined, <1 empty item> ] or in browser [2, 23, 7, 55, 7, 10, undefined, empty]
```   
**l) Array.prototype.splice (start, deleteCount [ , item1 [ , item2 [ , … ] ] ] )**: When this method is called with the three parameters **start**, **deleteCount** and **[ , item1 [ , item2 [ , … ] ] ] ** , **deleteCount**(number) elements of the array starting at array index *start* are replaced by the arguments *item1, item2*, etc. An Array object containing the deleted elements (if any) is returned.   
```
var arr = [4,2,6,99,12,10];
console.log(arr.splice(2,2,[44,21]));//[ 6, 99 ]
console.log(arr);//[ 4, 2, [ 44, 21 ], 12, 10 ]
arr.splice(2,1,44,21);
console.log(arr);//[ 4, 2, 44, 21, 12, 10 ]
//arr.splice(2,1,44,"a", {a:"obj"},, undefined);//SyntaxError non-existent elements cannot be added using splice
arr.splice(2,1,44,"a", {a:"obj"}, undefined);
console.log(arr);//[ 4, 2, 44, 'a', { a: 'obj' }, undefined, 21, 12, 10 ]
arr.splice(-1,3,9,0);//position is calculated same way its done for slice in case of negative start
console.log(arr);//[ 4, 2, 44, 'a', { a: 'obj' }, undefined, 21, 12, 9, 0 ]
```
**j) Array.prototype.unshift ( [ item1 [ , item2 [ , … ] ] ] )**:The arguments are prepended to the start of the array, such that their order within the array is the same as the order in which they appear in the argument list.  
```
var arr = [8,9,3];
arr.unshift(22,15);
console.log(arr);//[ 22, 15, 8, 9, 3 ]
```
**k) Array.prototype.indexOf ( searchElement [ , fromIndex ] )**: indexOf compares searchElement to the elements of the array, in ascending order, using the internal Strict Equality Comparison Algorithm, and if found at one or more positions, returns the index of the first such position; otherwise, -1 is returned.  
The optional second argument fromIndex defaults to 0 (i.e. the whole array is searched). If it is greater than or equal to the length of the array, -1 is returned, i.e. the array will not be searched. If it is negative, it is used as the offset from the end of the array to compute fromIndex. If the computed index is less than 0, the whole array will be searched.  
```
var arr = [2,3,1,55,3,67,890,34];
console.log(arr.indexOf(67));//5
console.log(arr.indexOf(67,0));//5
console.log(arr.indexOf(67,2));//5
console.log(arr.indexOf(67,-1));//-1 because element is searched starting from the last element of the array
console.log(arr.indexOf(67,-5));//5
```
**l) Array.prototype.lastIndexOf ( searchElement [ , fromIndex ] )**: lastIndexOf compares searchElement to the elements of the array in descending order using the internal Strict Equality Comparison Algorithm, and if found at one or more positions, returns the index of the last such position; otherwise, -1 is returned.  
The optional second argument fromIndex defaults to the array's length minus one (i.e. the whole array is searched). If it is greater than or equal to the length of the array, the whole array will be searched. If it is negative, it is used as the offset from the end of the array to compute fromIndex. If the computed index is less than 0, -1 is returned.
```
var arr = [2,3,1,55,3,67,890,67,34];
console.log(arr.lastIndexOf(67));//7
console.log(arr.lastIndexOf(67,0));//-1
console.log(arr.lastIndexOf(67,2));//-1
console.log(arr.lastIndexOf(67,-1));//7
console.log(arr.lastIndexOf(67,-5));//-1 
```
**j) Array.prototype.every ( callbackfn [ , thisArg ] )**:  
*callbackfn* accepts 3 arguments(*element* of current iteration, *index* - optional, index of current iteration element, *array* - optional, array on which *every* method is invoked) and returns `true` or `false`  
*every *calls *callbackfn *once for each element present in the array, in ascending order, until it finds one where *callbackfn *returns false. If such an element is found, *every *immediately returns false. Otherwise, if *callbackfn * returned `true` for all elements, *every *will return `true`. *callbackfn *is called only for elements of the array which actually exist; it is not called for missing elements of the array.  
```
var arr1 = [2,3,1,55,3,67,890,67,34];
var arr2 = [20,30,11,55,30,67,890,67,34];
function isGreaterThan10(element){
	console.log(element);
	return element > 10;
}
console.log(arr1.every(isGreaterThan10));
/*output
2
false
*/

console.log(arr2.every(isGreaterThan10));
/*
20
30
11
55
30
67
890
67
34
true
*/
```
Since the third parameter passed to the *callbackfn* is the array itself, the function will have access to the array and will be able to perform any operations that can modify it. *every* by itself does not mutate the array.  
**k) Array.prototype.some ( callbackfn [ , thisArg ] )**:  
The `some()` method tests whether *at least one element* in the array passes the test implemented by the provided function. It returns a Boolean value.  
**every** returns when the first negative result is hit and **some** returns when the first positive result is hit.  
```
var arr1 = [2,3,1,55,3,67,890,67,34];
var arr2 = [20,30,11,55,30,67,890,67,34];
function isGreaterThan10(element){
	console.log(element);
	return element > 10;
}
console.log(arr1.some(isGreaterThan10));
/*output
2
3
1
55
true
*/

console.log(arr2.some(isGreaterThan10));
/*
20
true
*/
```
**l) Array.prototype.forEach ( callbackfn [ , thisArg ] )**:  
*callbackfn* should be a function that accepts three arguments. `forEach` calls *callbackfn* once for each element present in the array, in ascending order. *callbackfn* is called only for elements of the array which actually exist; it is **not called for missing elements of the array**.  
If a `thisArg` parameter is provided, it will be used as the this value for each invocation of *callbackfn*. If it is not provided, `undefined` is used instead.  
*callbackfn* is called with three arguments: the value of the *element*, the *index* of the element, and the *object* being traversed.  
`forEach` does not directly mutate the object on which it is called but the object may be mutated by the calls to *callbackfn*.  
The range of elements processed by `forEach` is set before the first call to *callbackfn*. Elements which are appended to the array after the call to `forEach` begins **will not be visited** by *callbackfn*. If existing elements of the array are changed, their value as passed to callback will be the value at the time `forEach` visits them; elements that are deleted after the call to `forEach` begins and before being visited are not visited.  
```
var arr1 = [2,3,1,55,3,67,890,67,34];
arr1.forEach(function(element, index, arr){
	if(index%2 === 0){
		arr[index] += 1;//increments even indices by 1
	}
});
console.log(arr1);//[ 3, 3, 2, 55, 4, 67, 891, 67, 35 ]

arr1.forEach(function(element, index, arr){
	arr.pop();
});
console.log(arr1);//[ 3, 3, 2, 55 ]

arr1.forEach(function(element, index, arr){
	arr.splice(1,1);
});
console.log(arr1);//[ 3, 55 ]
```
**m) Array.prototype.map ( callbackfn [ , thisArg ] )**:  
*callbackfn* should be a function that accepts three arguments. map calls *callbackfn* once for each element in the array, in ascending order, and constructs a new Array from the results. *callbackfn* is called only for elements of the array which actually exist; it is not called for missing elements of the array.  
If a `thisArg` parameter is provided, it will be used as the this value for each invocation of *callbackfn*. If it is not provided, undefined is used instead.  
*callbackfn* is called with three arguments: the value of the *element*, the *index *of the element, and the *object* being traversed.  
map does not directly mutate the object on which it is called but the object may be mutated by the calls to *callbackfn*.  
The range of elements processed by map is set before the first call to *callbackfn*. Elements which are appended to the array after the call to `map` begins will not be visited by *callbackfn*. If existing elements of the array are changed, their value as passed to *callbackfn* will be the value at the time `map` visits them; elements that are deleted after the call to `map` begins and before being visited are not visited.  `map()` returns a new array.  
`map()` should not be used where an array is not expected to be returned. In those cases `forEach` should be used.  
```
var arr = [4,5,6,12,3,88,4,67,55,31];
var aThisObject = {
	a: 5
};
var x = arr.map(function(currentValue, index, arry){
	console.log(this);
	console.log(arry[index]);
	if(index%2 === 0){
		return arry[index] + this.a;
	}
	return arry[index]
},aThisObject);
console.log(x);//[ 9, 5, 11, 12, 8, 88, 9, 67, 60, 31 ]
```
**A tricky use case**- What could be the output of the below code?  
```
["1", "2", "3"].map(parseInt);
```
This behavior is due to the fact that the function `map()` passes 3 arguments(element, index, array) to the callback function and the function `parseInt()` takes 2 arguments(string, radix). So every iteration the index gets passed as the radix which causes the result.

**n) Array.prototype.filter ( callbackfn [ , thisArg ] )**:  
*callbackfn* should be a function that accepts three arguments and returns a value that is coercible to the Boolean value `true` or `false`. filter calls *callbackfn* once for each element in the array, in ascending order, and constructs a new array of all the values for which *callbackfn* returns true. *callbackfn* is called only for elements of the array which actually exist; it is not called for missing elements of the array.  
If a `thisArg` parameter is provided, it will be used as the this value for each invocation of *callbackfn*. If it is not provided, undefined is used instead.  
*callbackfn* is called with three arguments: the *value of the element*, the *index of the element, and the object being traversed. * 
filter does not directly mutate the object on which it is called but the object may be mutated by the calls to *callbackfn*.  
The range of elements processed by filter is set before the first call to *callbackfn*. Elements which are appended to the array after the call to `filter` begins will not be visited by *callbackfn*. If existing elements of the array are changed their value as passed to *callbackfn* will be the value at the time `filter` visits them; elements that are deleted after the call to filter begins and before being visited are not visited.  
```
var arr = [3,5,12,5,,77,33,45,22];
var filteredArray = arr.filter(function(ele,index,arry){
	return ele%2===0;
});
console.log(filteredArray);//[ 12, 22 ]
```

**o) Array.prototype.reduce ( callbackfn [ , initialValue ] )**:   
  This is the **left-to-right** version of the function, where values are reduced starting from the left side of the array.  
*callbackfn* should be a function that takes four arguments. reduce calls the callback, as a function, once for each element present in the array, in ascending order.  
*callbackfn* is called with four arguments: **the previousValue (or value from the previous call to callbackfn), the currentValue (value of the current element), the currentIndex, and the object being traversed.** The first time that callback is called, the *previousValue* and *currentValue* can be one of two values. If an *initialValue* was provided in the call to reduce, then *previousValue* will be equal to *initialValue* and *currentValue* will be equal to the first value in the array. If no *initialValue* was provided, then *previousValue* will be equal to the first value in the array and *currentValue* will be equal to the second.  
It is a `TypeError` if the array contains no elements and *initialValue* is not provided.  
Reduce does not directly mutate the object on which it is called but the object may be mutated by the calls to *callbackfn*.  
The range of elements processed by reduce is set before the first call to *callbackfn*. Elements that are appended to the array after the call to reduce begins will not be visited by *callbackfn*. If existing elements of the array are changed, their value as passed to *callbackfn* will be the value at the time reduce visits them; elements that are deleted after the call to reduce begins and before being visited are not visited.  
It is safer to provide an *initialValue*. (check  [here ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#Description) , how reduce used without an initialValue can be problematic)
> Syntax: arr.reduce(callback( accumulator, currentValue[, index[, array]] )[, initialValue])

```
var arr = [1,2,3,4];
var filteredArray = arr.reduce(function(previousValue, currentElement){
	return previousValue + currentElement;
});//reduce called without initialValue
console.log(filteredArray);//10

var filteredArray = arr.reduce(function(previousValue, currentElement){
	return previousValue + currentElement;
},5);//reduce called with initialValue
console.log(filteredArray);//15

reducedArray = arr.reduce(function(prev, curr){
	return Math.max(prev,curr);
});//reduce used to find the element with highest value
console.log(reducedArray);//4
```
    
**p) Array.prototype.reduceRight ( callbackfn [ , initialValue ] )**:   
This is the **right-to-left** version of the function, where values are reduced starting from the right side of the array.  
*callbackfn* should be a function that takes four arguments. *reduceRight* calls the callback, as a function, once for each element present in the array, in descending order.  
*callbackfn* is called with four arguments: **the previousValue (or value from the previous call to callbackfn), the currentValue (value of the current element), the currentIndex, and the object being traversed.** The first time the function is called, the *previousValue* and *currentValue* can be one of two values. If an *initialValue* was provided in the call to `reduceRight`, then *previousValue* will be equal to *initialValue* and *currentValue* will be equal to the last value in the array. If no *initialValue* was provided, then *previousValue* will be equal to the last value in the array and *currentValue* will be equal to the second-to-last value.  
It is a `TypeError` if the array contains no elements and *initialValue* is not provided.  
`reduceRight` does not directly mutate the object on which it is called but the object may be mutated by the calls to *callbackfn*.  
The range of elements processed by `reduceRight` is set before the first call to *callbackfn*. Elements that are appended to the array after the call to `reduceRight` begins will not be visited by *callbackfn*. If existing elements of the array are changed by *callbackfn*, their value as passed to *callbackfn *will be the value at the time `reduceRight` visits them; elements that are deleted after the call to `reduceRight` begins and before being visited are not visited.

### Properties of Array Instances###  
Array instances inherit properties from the Array prototype object and their [[Class]] internal property value is "Array". Array instances also have the following properties.  
**a) [[DefineOwnProperty]] ( P, Desc, Throw )**: we can defined new properties.  
**b) length**: The length property of this Array object is a data property whose value is always numerically greater than the name of every deletable property whose name is an array index.  
The length property initially has the attributes `{ [[Writable]]: true, [[Enumerable]]: false, [[Configurable]]: false }`.  
Attempting to set the length property of an Array object to a value that is numerically less than or equal to the largest numeric property name of an existing array indexed non-deletable property of the array will result in the length being set to a numeric value that is one greater than that largest numeric property name.  

Now we have learnt in depth about the properties that exist for the built-in function and array objects. In the next post we shall explore a few more built-in objects.
  
References:  
 [https://pvdz.ee/ecma/252](https://pvdz.ee/ecma/252)  
 [http://www.wirfs-brock.com/allen/posts/166](http://www.wirfs-brock.com/allen/posts/166) 