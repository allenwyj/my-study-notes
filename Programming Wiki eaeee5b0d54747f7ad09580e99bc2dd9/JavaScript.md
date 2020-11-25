# JavaScript

# JavaScript Basic

## Useful Codes

[exercise-Budgety.zip](JavaScript%20763ec93d44974cadb889821761607363/exercise-Budgety.zip)

### Random Number Generating

- `Math.random()` - Generate a number between [0 , 1)
- `Math.random() * 6` - Generate a number between [0 - 6)
- `Math.round(anInt)` -  returns the value of a number rounded to the nearest integer. 四舍五入
- `Math.ceil(anInt)` - 进一法
- `Math.floor(anInt)` - remove the decimal numbers
- `Math.floor(Math.random() * 6)` - [0, 5]

### Convert String to Number

- `parseInt(string [, radix])` - Convert to number
    - **always specify the radix.**
    - If the radix parameter is omitted, JavaScript assumes the following:
        - If the string begins with "0x", the radix is 16 (hexadecimal)
        - If the string begins with "0", the radix is 8 (octal). This feature is deprecated
        - If the string begins with any other value, the radix is 10 (decimal)
    - **Note:** Only the first number in the string is returned!
    - **Note:** Leading and trailing spaces are allowed.
    - **Note:** If the first character cannot be converted to a number, parseInt() returns NaN.
- `parseFloat()` - Convert to float

### indexOf()

- If `indexOf()`cannot found the associated item, it will return -1.
- `indexOf()` will only return the first element if it is matched.

### Formatting Number - Currency

- **regex -** `/(\d)(?=(\d{3})+(?!\d))/g`
    - `/.../` - indicates the start and the end of an regular expression and the second one also indicates the start of expression flag
    - `g` - retain the index of the last match, allowing iterative searches.
    - Look ahead positive `(?=)`
        - Find expression A where expression B follows:
            - `A(?=B)`
    - Look ahead negative `(?!)`
        - Find expression A where expression B does not follow:
            - `A(?!B)`
    - \B匹配非单词边界；\d匹配一个数字；+是量词，表示前面的内容重复1到多次?=是预言，表示这个位置后面的内容需要满足的条件，注意只是匹配一个位置，并不匹配具体的字符，所以是零宽；?!也是预言，表示这个位置后面的内容不能满足的条件，注意也只是匹配一个位置，并不匹配具体的字符，所以也是零宽；\d{3}匹配三个数字，+表示前面的内容重复1到多次，所以(\d{3})+表示三个数字的1到多次，也就是3,6,9...等3的倍数个数字的字符串；(?!\d)匹配一个位置，这个位置后面不是数字(?=(\d{3})+(?!\d))匹配一个位置，这个位置后面首先是3的倍数个数字的字符串，接下来的位置不是数字/\B(?=(\d{3})+(?!\d))/g就是全局匹配一个位置，这个位置是非单词边界，然后后面是3的倍数个数字，然后是非数字。比如，字符串ad12345678abs，这个正则匹配的位置就是2后面的位置，5后面的位置。2后面有6（3 * 2）个数字，5后面有3（3 * 1）个数字。

```jsx
var formatNumber = function(num, type) {
    var numSplit, int, dec;

    // returning an absolute number
    num = Math.abs(num);
    // giving 2 decimal numbers and return its value as a string
    num = num.toFixed(2);

    numSplit = num.split('.');
    int = numSplit[0];

    // adding thousand seperator
    /**
    *if(int.length>3){
    *int = int.substr(0,int.length-3)+','+int.substr(int.length-3,3);
    *}
    **/
    int = int.replace(/(\d)(?=(\d{3})+(?!\d))/g,'$1,');

    dec = numSplit[1];

    // adding '+' or '-' 
    type === 'exp' ? sign = '-' : sign = '+';

    return sign + ' ' + int + '.' + dec;
};
```

### Getting URL from the Browser

```jsx
// monitor if the hash value is changed.
window.addEventListener('hashchange', callbackFunc);
// getting url from the browser
const url = window.location;
// getting the hash value
const id = window.location.hash;
```

## Adding JS File

```html
<body>
	...
	<script src="app.js"></script>
</body>
```

## Variable

- `var` is **function-scoped**. Because `var`  allows the compiler to create the variable to `undefined` in the creation phase (**Hoisting**)

    **变量提升：**

    [浅谈JS变量提升_技术之路-CSDN博客](https://blog.csdn.net/qq_39712029/article/details/80951958)

- For example,
    - if `isTrue = true`，then console will print `3`.
    - if `isTrue = false`, then console will print `undefined`.

    ```jsx
    var firstName = 'John';

    function thisFunction(isTrue) {
    	if (isTrue) {
    		var a = 1;
    	}
    	console.log(a); // a can be accessed 
    }
    ```

### Naming Rules

- Best practice: using **camelCase**
- Cannot start a variable name with numbers or symbols, excepts
    - can only use _ (underscore), $ (dollar sign) and letter as the first letter.
        - _age, $age

---

## Data type

- JavaScript has **dynamic type**: data types are automatically assigned to varibales.

### Primitive JavaScript Data Types

In JavaScript, a primitive (primitive value, primitive data type) is data that is **not an object** and has **no methods**.

- number
    - Floating point numbers, for decimals and integers
    - `var age = 18;`
- string
    - Sequence of characters, used for text
    - `var name = 'Wu';`
- boolean
    - Logical data type that can only be **true** or **false**
    - In boolean expression, `1` is `true`, `0` is `false`
    - `var isTure = true;`
- bigint
    - provides a way to represent whole numbers **larger than 2^53 - 1**
- symbol
    - A “symbol” represents a unique identifier.
- undefined
    - data type of a variable that does not have a value yet
    - `var undefinedVar;`
- null
    - also means 'non-existent'
    - `var nullVar = null;`
    - which is **seemingly** primitive, but indeed is **a special case** for every Object: and any structured type is derived from null by the Prototype Chain.

### Primitive wrapper objects in JavaScript

Except for `null` and `undefined`, all primitive values have object equivalents that wrap around the primitive values:

- `[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)` for the string primitive.
- `[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)` for the number primitive.
- `[BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)` for the bigint primitive.
- `[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)` for the boolean primitive.
- `[Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)` for the symbol primitive.

The wrapper's `[valueOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)` method returns the primitive value.

---

## Comment

- single line comment
    - `// this is single line comment`
- multiple lines comment

    ```jsx
    /**
    * This is multiple lines comment
    */
    ```

---

## Variable Mutation and Type Coercion

### Type Coercion

- JavaScript automatically converts types from one to another as it's needed

    ```jsx
    var firstName = 'Allen';
    var age = 25;

    console.log(firstName + ' ' + age); // Allen 25
    ```

### Variable Mutation

- Can change the variable type as needed even though it's already initiated.

    ```jsx
    var job, isStudent;

    job = 'student';
    isStudent = true;

    // later
    job = 10; // can change the type.

    // pop up
    alert(job);

    // prompt a window to ask user input
    var lastName = prompt('What is your last name?');
    ```

## Basic Operators

### math operators

- `+ - * /`

### logical operators

- `> <  >= <=`

### typeof operator

```jsx
console.log(typeof age); // result: number
```

### More Operators

```jsx
x *= 2;
x += 2;
x -= 2;
x /= 2;
x += 1; x = x + 1; x++; // they are the same.
```

---

## Operator Precedence

### Precedence

- Assignment: From right-to-left
- [Operator Precedence List](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

```jsx
var now = 2020;
var yearAllen = 1995;
var fullAge = 18;

var isFullAge = now - yearAllen >= fullAge; // addition is higher than comparison
console.log(isFullAge); // result: true
```

### Muptiple Assignments

- assign value to multiple variables at the same time.

    ```jsx
    var x, y;
    x = y = 8; // beacause assignment presendence is right-to-left. 
    ```

---

## If-else Statements

### With Ternary Operator

```jsx
// ternary operator
var age = 16;
age >= 18 ? console.log('true, do sth') : console.log('false, do sth'); // false
```

### Switch Statement

- **If you omit (省略) the `break` statement, the next case will be executed even if the evaluation does not match the case.**
- If `default` is not the last case in the switch block, remember to end the default case with a break.
- If we are using `return` statement, then we dont need to use `break`.

```jsx
// Switch statement
var job = 'teacher';
switch (job) {
	case 'teacher' : // no break, after finishing the code inside teacher case
	case 'instructor': // this case will execute no matter it matched or not
			console.log('');
			break;
	case 'driver' :
			console.log('');
			break;
	// ...
	default :
			console.log('');
			// no need a break statement, if it's the last case
}

// using switch statement for a range comparation
switch (true) {
	case age < 13:
			//...
			break;
	case age >= 13 && age < 20:
			//...
			break;
	case age >= 20:
			//...
			break;
	default:
			// do something.
}
```

---

## Truthy and Falsy Values and Equality Operators

### Falsy Value

- **Falsy** values: undefined, null, **0**, ''(empty string), NaN
- Falsy value is considered as false in the if-else statement

### Truthy Value

- **Truthy** values: NOT falsy values
- Truthy value is considered as true in the if-else statement

```jsx
var height;
// height = 18; // if height is equal to 0, it will go to else statement.
// if (height || height === 0) {
if (height) {
	console.log('Variable is defined');
} else {
	console.log('Variable has NOT been defined');
}
```

### Common Practice to Check if Undefined Value Exists

- `if (height || height === 0) {...}`

### Equality Operators

- == equal to
    - does type coercion → value should be the same, type can be different
- === equal value and equal type

```jsx
// ==: equal to
var x = 5;
if (x == 8) {...} //false
if (x == 5) {...} // true
if (x == '5') {...} // true

// ===: equal value and equal type
if (x === '5') {...} // false

// boolan value
var isTrue = true;
if (isTrue) {...} 

// comparison
if (a > b) {
	...
} else if {
	...
} else {
	...
}
```

---

## **Function Statements and Expressions**

### Method

- A JavaScript method is a property containing a function definition.
    - The `fullName` property will execute (as a function) when it is invoked with `fullName()`.
    - If you access the `fullName` property, without `()`, it will `return` the function definition:

    ```jsx
    var person = {
      firstName: "John",
      lastName : "Doe",
      id       : 5566,
      fullName : function() {
        return this.firstName + " " + this.lastName;
      }
    };
    ```

### Statement

- anything that are not executed immediately is a **statement**. (saved for later use)

    i.e. if..else statements, switch statements etc.

### Expression

- Statements involving functions which do not start with function are **function expressions**.
- anything that we do produces a result is an **expression**.

    i.e. 2+3, age > 10 etc. if(a > b) 中的a>b是一个expression, or **variable assignment**

```jsx
// function declaration
function whatDoYouDo(job, firstName) {
	// ...
}

// function expression
var whatDoYouDo = function(job, firstName) {
	switch(job) {
		case 'teacher':
			return fristName + ' teaches kids how to code';
		case 'driver':
			return firstName + ' drives a cab in Melbourne';
		case 'designer':
			return firstName + ' designs beautiful websites';
		default:
			return firstName + ' does something else';
	}
};

console.log(whatDoYouDo('teacher', 'John')); 
```

---

## Arrays

- array will have Array as _proto_

    ![JavaScript%20763ec93d44974cadb889821761607363/Untitled.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled.png)

- otherwise, it is an array-like object.

    ![JavaScript%20763ec93d44974cadb889821761607363/Untitled%201.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%201.png)

- A collection of variables which can have different data types.

```jsx
// Initialize new array.
var names = ['a', 'b', 'c'];
var years = new Array(1990, 2000, 2010);

// different types
var i = ['allen', 25, true];

// mutate the array
names[1] = 'S';
// ['S','b','c'];

names.push('') // add the value to the end of the array, return **length** property
names.unshift('') // add the value to the start of the array, return **length**
names.pop() // remove the last element. return the removed element
names.shift() // remove the first element. return the removed element
names.indexOf('a') // return the index. if there is no the matched value in the array, 
										//**will reutrn -1**
```

### slice() - Get part of an Array or Convert A List To An Array

`let newArray = arr.slice([start[, end]])`

- **return the sliced item(s) to a new array**
- we can use the `slice(start, end)` method - **[start, end)**, which is a build-in method in Array. (list doesn't have forEach method)
    - The `slice()` method returns the selected elements in an array, as a new array object.
    - The `slice()` method selects the elements starting at the given *`start`* argument, and ends at, ***but does not include*, the given *`end`* argument.**
    - The `slice()` method will return the whole array into a new array if start and end are **NOT** given.
    - **Note:** The original array will not be changed.
- **Convert List to Array**
    - But we cannot directly do something like this → `aList.slice()`
    - Because `slice()` belongs to Array and its object, so we can access it by calling **`Array`** and we need to perform **method borrowing**

        ```jsx
        function list() {
          return Array.prototype.slice.call(arguments)
        }

        let list1 = list(1, 2, 3) // [1, 2, 3]
        ```

        - `var fieldsArr = Array.prototype.slice.call(fields);`

### splice() - Add or Delete Item From Array

`let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

- **return the removed item(s) and mutate the array.**
- `array.splice(index, 1);`
    - **Remove** `1` elements at the `index`.
    - index: An integer that specifies at what position to add/remove items, Use negative values to specify the position from the end of the array.
        - starting from the index to perform the action.
    - 1: the number of items you want to delete.
- `array.splice(index, 0, item);`
    - **Add** `item` before the `index` position.
- `array.splice(index, 1, item);`
    - **Replace** `item` at the `index` position

```jsx
// 仅删除第一个匹配项
function removeItemOnce (arr, value) {
  let index = arr.indexOf(value);
  if (index > -1) {
    arr.splice(index, 1);
  }
  return arr;
}

// 删除所有匹配项
function removeItemAll (arr, value) {
  let i = 0;
  while(i < arr.length) {
    if (arr[i] === value) {
      arr.splice(i, 1); // 删除数组中索引i处的元素
    } else {
      ++i;
    }
  }
}
```

- **NOTE: `splice()` will modify the array and the index of the rest elements of the array will be changed as well. In case to remain the same index, use `delete` operator instead:**

    `delete exporession`

    - return `true` else `false` to indicate the successful deletion

    ```jsx
    const array = [1, 2, 3, 4];

    console.log(array);
    // expected output: [1, 2, 3, 4]

    delete array[2];

    console.log(array);
    // expected output: [1, 2, undefined, 4]
    ```

### concat() - Merge two or more arrays

`const new_array = old_array.concat([value1[, value2[, ...[, valueN]]]])`

- return a new array.
- Does not change the existing arrays
- `const newArray = array1.concat(array2, array3, ...);`
- Concatenating values to an array
    - `const newArray = array1.concat(1, [2, 3]);`

### join() - Join Items Together As a string

- `arr.join([separator])`
    - `separator` is optional
- The `join()` method creates and returns a new string by concatenating all of the elements in an array (or an array-like object), separated by commas or a specified separator string. If the array has only one item, then that item will be returned without using the separator.

## **Objects and Properties**

### Create Object

```jsx
// Create a new object
var john = {
	firstName: 'John',
	lastName: 'Smith',
	birthYear: 1990,
	family: ['Jane', 'Mark', 'Bob', 'Emily'],
	job: 'teacher',
	isMarried: false,
};

// OR
var jane = new Object();
jane.firstName = 'Jane';
// ....
```

### Access the Object and Its Properties

```jsx
// accessing the object
console.log(john);
// accessing the specific element, by using keys or object['keyName']
console.log(**john.firstName**);
console.log(**john['firstName']**);
```

### Mutate the Object

```jsx
// change the field
john.job = 'designer'; 

// OR
john['isMarried'] = true;
```

### Object and Methods

```jsx
var john = {
	... // other properties
	birthYear: 1995; 
	
	// calling method by passing a parameter 
	calcAge: function(birthYear) {
		return 2020 - birthYear;
	},
	// Using object field.
	objectMethodB: function() {
		return 2020 - this.birthYear; 
	},
	// Calling method and saving to object field.
	objectMethodC: function() {
		this.age = 2020 - this.birthYear; 
	}
};

console.log(john.calcAge(2020));
console.log(john.objectMethodB());
john.objectMethodC();
```

---

## **Loops**

- `continue`: break the current iteration and do the next
- `break`: break the loop at all.

```jsx
var john = ['a', 'b', 'c'];
  
// for loop
for (var i = 0; i < 10; i++) {
  // might be better than having inside if.
	if (typeof john[i] !== 'string') continue;
	console.log(john[i]);
}
for (var i = 0; i < john.length; i++) {
	console.log(john[i]);
}

// interation
for (var i in arry) {
	...
}

// **forEach method
// the function is requried
	// currentValue	Required. The value of the current element
	// index	Optional. The array index of the current element
	// arr	Optional. The array object that the current element belongs to
// thisValue	- Optional. Value to use as this when executing the callback.
// If this parameter is empty, 
// the value "undefined" will be passed as its "this" value**
// **the function can receive up to 3 arguments.**
array.forEach(function(currentValue, index, arr), thisValue)

// while loop
var i = 0;
while(i < john.length) {
	...
	i++;
}
```

### forEach() Method

`array.forEach(function(currentValue[, index[, arr]])[, thisValue])`

- `forEach` will return each element inside the array as the parameter to the callback function)
- **callback** **function** is **requried**
    - **currentValue**
        - **Required**. The value of the current element
    - **index**
        - **Optional**. The array index of the current element
    - **arr**
        - **Optional**. The array object the current element belongs to
- **thisValue**
    - **Optional**. A value to be passed to the function to be used as its "this" value. If this parameter is empty, the value "undefined" will be passed as its "this" value

### map() Method

`let aNewArr = array.map(function(currentValue[, index[, arr]])[, thisValue])`

- so `map()` will loop the array, and pass the current value into the callback function.
- The callback has to **return** something and put it into a new array based on the old array index.
- `**return` statement in the callback is necessary.**
- **example**: we can use indexOf to get the actual index if we wanna use the id to search.
    - NOTE: if indexOf() cannot found the associate item, it will return -1.

```jsx
// mapping all elements in the arrays, and
// storing all of its id following its element index.
var ids = data.allItems[type].map(function(current) {
   **return current.id;** // return to ids array
});
```

### Difference Between forEach and map Method

- `map()` is faster than `forEach()` method
- `forEach()` affects and changes our original Array, whereas `map()` returns an entirely new Array — thus leaving the original array unchanged.
- `map()` allocates memory and stores `return` values. `forEach()` throws away return values and always returns `undefined`.
- `forEach()` will allow a callback function to mutate the current array. `map()` will instead return a new array.

### Custom forEach Method For List Object

```jsx
/** loop over the nodeList, and for each item in the list
	* performs the business logic in our callback function.
	* callback function has currentElement and its index as the arguments. 
	**/
var nodeListForEach = function(list, callback) {
    for (var i = 0; i < list.length; i++) {
        callback(list[i], i);
    }
};

nodeListForEach(fields, function(current, index) {
    // our business logic
});
```

---

# Execution Contexts

## Global Execution Context

- Code that is **not inside any function**
- Associated with the global object
- In the browser, that's the window object

    `lastName === window.lastName;`

## Execution Context Object

### Creation Phase and Execution Phase

1. Creation phase

    1. Creation of the Variable Object(VO)

    2. Creation of the scope chain

    3. Determine value of the 'this' variable

2. Execution phase

    - The code of the function that generated the current execution context is ran line by line.

### Variable Object(VO) - Hoisting

- contains function arguments in an **variable declaration** and **function declaration**.
- the argument object is created, containing all the arguments that were passed into the function
- ***Hoisting***

    意思：js 会在run成功时 (creation phase)，先 go through所有代码，对function declaration进行defined；**对变量进行生成，但不赋值**先（undefined），然后**在运行阶段时，根据代码顺序，进行赋值**。

    - They are available before the execution phase start. ( it means we don't have to declare functions, then call the functions.)
    - Code is scanned for **function declarations**: for each function, a property is created in the Variable Object, **pointing to the function.**
        - **Functions are already defined before the execution phase starts**
        - So, we don't have to declare the function first then use it.
            - But, if we are using function expression, then the order becomes important.
    - Code is scanned for **variable declarations**: for each variable, a property is created in the Variable Object, and set to **undefined**.
        - Variables are created and set up to 'undefined' and will only be defined in the execution phase. i.e. `var age = 18;`

### Scope Chain

- contains the current variable objects as well as the variable objects of all its parents.
- Each new function creates a scope
    - the space/environment, in which the variables it defines are accessible.
- Lexical scoping
    - a function that is lexically within another function gets access to the scope of the outer function.

### 'This' Variable

**Normal Situation - Not using arrow function**

- In a method, `this` refers to the **owner object.**
- In a function, `this` refers to the **global object**.
- In a function, **in strict mode**, `this` is `undefined`.
- In an event, `this` refers to the **element** that received the event.
- Alone, `this` refers to the **global object**

---

- Regular function call
    - the 'this' keyword points at the global object, (the window object, in the browser);
    - even though an regular function call is inside another function or method, it will point to the global object → window object. - see the example 2
- Method call
    - the 'this' keyword points to the object that is calling the method.
- The 'this' keyword is not assigned a value until a function where **(which object)** it is defined is actually called.

---

**Arrow Function**

- An arrow function does not have its own this. The this value of the enclosing lexical scope is used; arrow functions follow the normal variable loopup rules.
    - 箭头函数没有自己的this值
    - 箭头函数中使用的this都是来自函数的作用域链
    - 取值遵循普通变量一样的规则，在函数作用域链中一层一层往上找

**Rules to follow to avoid this problem**

- 对于需要使用object.method()方式调用的函数，使用普通函数定义，不要使用箭头函数。对象方法中所使用的this值有确定的含义，指的就是object本身。
- 其他情况下，全部使用箭头函数。

---

```jsx
// 1.
calculateAge(1985);
// a regular function
function calculateAge(year) {
	console.log(this); // this - window object
}

// 2.
var john = {
	name: 'john',
	yearOfBirth: 1995,
	
	// var that = this;
	calculateAge: function() { 
		console.log(this); // this: 'john' object
		console.log(2016 - this.yearOfBirth);
		
		**// it is still a regular function.**
		**function innerFunction() {
			console.log(this); // this: window object.
		}**
		innerFunction();
	}
};

john.calculateAge();
**// so we can use var that = this; to replace all this in side the inner
// function or we can bind the inner function to avoid this problem
// or we can use arrow function inside the method.**

var mike = {
	name: 'Mike',
	yearOfBirth: 1984
};

**// method borrowing**
mike.calculateAge = john.calculateAge;
// 'this' will refer to the object as soon as it gets called.
mike.calculateAge();  
```

Calling functions.

```jsx
function btn() {
    
}

btn(); // That's how we call function by us

// we want eventlistener call this function for us.
document.querySelector('.btn-roll').addEventListener('click', btn); // callback function, the function we pass to another function as an aurgument.
```

---

# Advanced JavaScript

## Getting Element From HTML

### querySelector

- we can select the element by ID or Class Name using CSS way. (using # or .XXX)
- `document.querySelector()`
    - `document.querySelector('h1');` - getting h1 tag
    - `document.querySelector('.className');`
    - `document.querySelector('#score-0')` - using id
    - `document.querySelector('#current-' + activePlayer)` - more dynamic
- `textContent` - can only add String value
    - Reading value from the text content

        `var x = document.querySelector(...).textContent`;

    - Writing value to the text content

        `document.querySelector(...).textContent = dice`;

- Add HTML codes.
    - `document.querySelector(...).innerHTML`
- Control the CSS code
    - `document.querySelector('.dice').style.display = 'none';` - hide the property

### getElementById

- This can only work for select an element by using its ID (we dont need to use .XXX)
- It is faster than the querySelector
- `document.getElementById('score-0').textContent = '0';`

### Add, Remove Element From DOM - Modify from its Parent

- we can perform this by using element's element id
- First, we need to find the element which we want to delete
- Then, we get its **parent class**, and delete the element from the **parent** class.

`var element = document.getElementById(selectorID);`

- **Add - appendChild()**

    `document.getElementById('parent').appendChild(element);`

- **Remove - removeChild()**
`element.parentNode.removeChild(element);`

### Add, Remove Element's CSS Class

[Element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)

- `document.querySelector('.className').classList.add('className');`
- `document.querySelector('.className').classList.remove('className');`
    - `remove` won't cause any error even though there is no the removed className.
- **Toogle**
    - If a class is there, then remove, if a class is not there, then add the class.
    - `document.querySelector('.className').classList.toogle('className');`

### Element Contains A Class

`document.querySelector('.className').classList.contains('className');`

- `Element.classList` - returns a **DOMTokenList** which has `contains()` method.
- return `ture` or `false`;

`document.querySelector('.className').className.includes('className');`

- `Element.className` - returns a space-delimited **string**.
- return `ture` or `false`;

### Reading User Input Box Value

- `var input = document.querySelector.('.final-score').value;`

### Clear HTML Fields

```jsx
// clear the user input field to empty after user hits enter or 'save'
clearField: function() {
    var fields, fieldsArr;
    
    // querySelectorAll fetches the HTML elements that we want
    // and store them into a list
    fields = document.querySelectorAll(DOMStrings.inputDesc + ',' + DOMStrings.inputValue);
    // convert the list into an Array
    fieldsArr = Array.prototype.slice.call(fields);
    
    // the elements of fields or fieldsArr are references which points to the same location
    // so we can set them to empty string "", to change the HTML (clear fields)
    fieldsArr.forEach(function(currentValue){
        currentValue.value = "";
    });
    
    // focus on the description field after saving.
    fieldsArr[0].focus();
}
```

### Use querySelectorAll

`var fields = document.querySelectorAll(element1 **+ ',' +** element2);`

- It can select multiple elements in the HTML at once, simply using CSS combine rule `(xxx + ',' + xxx)` and **return a Array-like list.**
- will return a nodeList **NOT** an array, so we need to convert the list to an array which has more methods.
    - **Both the nodeList and the converted array are storing the same element which is the element from HTML. They are pointing to the same HTML element.**

### Convert A List To An Array - using slice()

- we can use the `slice(*start*, *end*)` method which is a build-in method in Array.
    - The `slice()` method returns the selected elements in an array, as a new array object.
    - The `slice()` method selects the elements starting at the given *`start`* argument, and **ends at, *but does not include***, the given *`end`* argument.
    - The `slice()` method will return the whole array into a new array if start and end are **NOT** given.
    - **Note:** The original array will not be changed.
    - But we cannot directly do something like this → `fields.slice()`
    - Because `slice()` belongs to Array and its object, so we can access it by calling **`Array`** and we need to perform **method borrowing**
- `var fieldsArr = Array.prototype.slice.call(fields);`
- `var fieldsArr = Array.prototype.slice.call(fields, 1);`
    - start copying from the index 1.

## Events and Event Handling

### Events

- Notifications that are sent to notify the code that something happened on the webpage.
    - i.e. clicking a button, resizing window, scrolling down or pressing a key

### Event Listener - addEventListener()

- **[Event Type Reference](https://developer.mozilla.org/en-US/docs/Web/Events#Most_common_categories) ← Click to check**
- A function that performs an action based on a certain event. It waits for a specific event to happen.
- `.addEventListener()`
    - Has two parameter
        - event type
        - callback function
            - **the listener will pass a argument `event` to the callback function.**
            - the called function name **without the bracket.**
                - because we want the event listner call this function for us.
    - `document.querySelector('.btn-roll').addEventListener('click',btn);`
    - `btn` now becomes a call-back function, OR
    - `function(){...}` instead of btn - now becomes an anonymous function
        - doesn't have a name, so it cannot be reused.

        ```jsx
        document.querySelector('.btn-roll').addEventListener('click', function() {
        	// Do something here, only works when users click the button.
        });

        // adding a global event listener, for example key press
        document.addEventListener('keypress', function(event){

        	// which property is for more older browser
        	if (event.keyCode === 13 || event.which === 13) {

        		console.log('Enter is pressed');
        	}

        });
        ```

### Event Bubbling

- Event bubbling means that when an event is fired or triggled on some DOM element, then the exact same event is also triggled on all of the parent elements.
- Target Element
    - the first fired event

### Event Delegation

**Attach an event handler to the parent element of our interested element, and catch the event there when it bubbles up**

- 因为我们不可以对一个还不存在的HTML 元素进行event listener的监听，所以我们改为对我们感兴趣的元素的母元素进行一个监听，然后通过对其内部的点击时，返回的一个event object中，我们可以知道点击的target是哪里， 从而对我们感兴趣的元素进行操作。
- If the event bubbles up in the DOM tree, and if we know where the event was fired, we can simply attach an event handler to a parent element and wait for the event to bubble up.
- then perform the action that we want to do with our target element.
- For example
    - when we have an element with lots of child elements that we are interested in,
    - **when we want an event handler attached to an element that is not yet in the DOM when our page is loaded.**
    - 当用户想要点击删除某行记录的时候，我们可以在需要监听的container中addEventListener来知道用户点击的是否是监听的container以及点击的是哪一个element以及它的id。 通过DOM Traversing，我们可以操控它的parent element删除这个被点击的child element。

### DOM Traversing - using parentNode property

- Using `parentNode` to get the parent of the target element in the HTML, so we can perform the event delegation.
- Hard coded the DOM structure here: (NOT A GOOD PRACTICE) `event.target.parentNode.parentNode.parentNode.id` - getting 3 upper levels parent's id
    - `event` is from adding the event listener which is listening the whole container which satisfies our needs.

### DOM Traversing - using Element.closest()

- The `closest()` method **traverses** the Element and its **parents** (heading toward the document root) until it finds a node that matches the provided selector string.
- **Will return itself or the matching ancestor.** If **no** such element exists, it returns **null**.
- 我们可以通过设定对我们感兴趣的元素，如果溯源后找到的是我们感兴趣的class，我们就可以获得这个元素。用条件判断，来操作我们的需求（找到了，做什么。没找到，就不做）。因为不知道会点在哪一个子层上。

```jsx
elements.shoppingList.addEventListener('click', e => {
    const id = e.target.closest('.shopping__item').dataset.itemid;
});
```

### DOM Traversing - using Element.matches()

- "The matches() method checks to see if the Element would be selected by the provided selectorString -- in other words -- checks if the element "is" the selector."
- `var result = element.matches(selectorString);`

```jsx
// check if the click is on the button or its child elements
if (e.target.matches('.btn-decrease, .btn-decrease *')) {
    // do sth
}
```

### Event Common Methods

`preventDefault()`

- "The Event interface's preventDefault() method tells the user agent that if the event does not get explicitly handled, its default action should not be taken as it normally would be."

    ```jsx
    document.querySelector('.search').addEventListener('submit', e => {
        e.preventDefault();
    });
    ```

# Objects and Functions

### Everything Is An Object

**Primitives**

- Numbers
- Strings
- Booleans
- Undefined
- Null
- bigint
- symbol

**Primitive variable contains the values in the variable.**

**Everything else is an object**

- Arrays
- Functions
- Objects
- Dates
- Wrappers for Numbers, Strings, Booleans

**Variable only contains the reference which points to the object.**

```jsx
var age = 30;
var obj = {
	name: 'abc',
	city: 'Guangzhou'
}

function change(a, b) {
	a = 20;
	b.city = 'Melbourne';
}

change(age, obj);
// age will remain 30, because age is primitive type,
// it will **copy the value** and paste it to 'a'.
// city will chagne to 'Melbourne' because when function
// is passing its parameter, it will **copy its reference.**
```

- If we define a field with the same key name which already exists in the object or it is defined before, then its value will be overwritten by the later one.

    ```jsx
    const obj = {
    	a : 1,
    	b : 2,
    	c : 3,
    	b : 6 // overwrite the value of b => {a:1,b:6,c:3}
    }
    ```

### Return As An Object

```jsx
function() {
            
    return {
        type : document.querySelector('.add__type').value,
        description : document.querySelector('.add__description').value,
        value : document.querySelector('.add__value').value
    }
}
```

# Class - Function Constructors and Instances in JavaScript

- This is recognised as Class in other OO programming languages.

## Factory Function

- A **factory function** can return an object without using `new` keyword. Basically, it is a function that **receives** some arguments, **creates** a new object, **attaches** these arguments as the property of the object and **returns** the object.

```jsx
function person(firstName, age) {
	**const person = {};**
	person.firstName = firstName;
	person.age = age;

	**return person;**
}
```

![JavaScript%20763ec93d44974cadb889821761607363/Untitled%202.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%202.png)

## Constructor Function

- Function name has to be **capitalised** to clearify that the function is a constructor function.

```jsx
function Person(firstName, age) {
	this.firstName = firstName;
	this.age = age;
}
```

![JavaScript%20763ec93d44974cadb889821761607363/Untitled%203.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%203.png)

## new Keyword

*The **`new` operator** lets developers create an instance of a user-defined object type or of one of the built-in object types that has a constructor function.*
More info : MDN Documentation `new` operator

- Firstly, we need to know about `new` operator and how it runs behind the scene.

    The commented codes are JS engine does for us.

    ```jsx
    function Person(firstName, lastName, age) {
        // this = {};
        // this.__proto__ = Person.prototype;
        // Set up logic such that: if there is a return statement
        // in the function body that returns anything **EXCEPT an
        // object, array, or function**:
        //  return **this** (the newly constructed object) instead of that item at
        //  the return statement;
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        // return this;
    }
    ```

![JavaScript%20763ec93d44974cadb889821761607363/Untitled%204.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%204.png)

Green Arrow: `functionName.prototype`

Blue Arrow: `functionName.prototype.constructor`

### with or without new keyword

![JavaScript%20763ec93d44974cadb889821761607363/Untitled%205.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%205.png)

## Create Object by Constructor Function

- **Constructor (Popular way)**
    - Used to **create objects** (Instances)

    ```jsx
    function Person(name, yearOfBirth, job) {
        this.name = name;
        this.yearOfBirth = yearOfBirth;
        this.job = job;
    		// If we create a function as a property 
    		// inside the constructor, it becomes a prototype function.
    		/*
    			this.calculateAge = function() {
    		   console.log(2016 - this.yearOfBirth);
    			};
    		*/
    };

    // **OR

    var Person = function(name, yearOfBirth, job) {
    		this.name = name;
    		this.yeaerOfBirth = yearOfBirth;
    		this.job = job;**
    }

    var allen = new Person('Allen', 1995, 'student');
    ```

    - **the prototype of allen is the prototype property of the Person function constructor.**
    - **the prototype of the Person is the prototype property of the Object function constructor.**

        ![JavaScript%20763ec93d44974cadb889821761607363/Untitled%206.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%206.png)

        ![JavaScript%20763ec93d44974cadb889821761607363/Untitled%207.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%207.png)

    **Person.prototype.methodName**

    - If we create a function as a property inside the constructor, every time when we **create a new object**, we will **create a new function** to attach to this new object. - **NOT A GOOD PRACTICE**
    - So, instead of putting this method in the constructor, it's better to **create the method inside the Person prototype,** so every new object can **inherit** this method - pointing to the same memory location.

        ```jsx
        Person.prototype.calculateAge = function() {
            console.log(2016 - this.yearOfBirth);
        };

        Person.prototype.lastName = 'Every object will have this property with the same value.';
        ```

- **Object.create (Handy way in some cases)**
    - The `Object.create()` method creates a new object, using an existing object as the prototype of the newly created object.
    - it can be used to create the prototype for an object

        ```jsx
        // Object.create
        var personProto = {
        		calculateAge: function() {
        				...
        		}
        };

        // by passing one parameter**(not an ideal way)**
        var john = Object.create(personProto);
        john.name = 'Allen';
        john.yearOfBirth = 1999;

        // by passing two parameters
        var john = Object.create(personProto, 
        {
        	name: { value: 'Allen'},
        	yearOfBirth: { value: 1999}
        });
        ```

    - Benefit by using `Object.create`
        - It can help to build a realy complex inheritant structures in an easier way than using function constructor.
        - Because it allows to directly specify which object should be a prototype.
- **Prototype Chain**
    - Every JavaScript object has a **prototype property,** which makes inheritance possible in JavaScript.
    - The protype property of an object is where we put methods and properties that we want **other objects to inherit**
    - The Constructor's protype properly is **NOT** the prototype of the Constructor itself, it's the prototype of **ALL** instances that are created through it.
    - When a certain method (or property) is called, the search starts in the object itself, and if it cannot be found, the search moves on to the object's prototype. This continues until the method is found: **prototype chain.**

### Inheritance

- prototype makes inheritance possible in JavaScript
- **the methods and properties which we want to inherit will be placed into the prototype**
- Each object that we created is inherited from the Object object.

    ![JavaScript%20763ec93d44974cadb889821761607363/Untitled%208.png](JavaScript%20763ec93d44974cadb889821761607363/Untitled%208.png)

# Functions

Function are also objects in JavaScript

- return function needs to be set to a variable
    - `var functionName = function() { return 'helloworld';};`
    - if return is another function, we can call it without saving it as another variable.

        `function myFunction() { return function() {...};}`

        `myFunction()();` can call it.

- A function is an instance of the Object type
- a funtion behaves like any other object
- we can store functions in a variable
- we can pass a function as an argument to another function
    - when we define it
        - `function usingFunction(fn) { fn(); }`
            - `fn` just the name of an argument.
    - when we call it (fn is a call-back function, usingFuction() will call it for us)
        - `usingFunction(functionName);`
- we can return a function from a function.

    ```jsx
    function interviewQ(job) {
    	if (job === 'designer') {
    		return function(name) {
    			console.log(name);
    		}
    	} else if (job === 'teacher') {
    			return function(name) {
    				...
    			}
    	} else {
    		return function(name) {
    			...
    		}
    	}
    }
    var teacherQuestion = interviewQ('teacher');
    teacherQuestion('john'); // passing **name** variable 
    ```

## Immediately Invoked Function Expressions (IIFE) - 函数的自我调用

- Defined a function and immediately call it without calling its name
    - everything inside a parenthesis cannot be a statement, it should be treated as a statement.

    ```jsx
    (function () {
    	var socre = Math.random() * 10;
    	console.log(score >= 5);
    })();
    ```

- O**btain data privacy and also don't interfere with other variables.**
    - we can place all our codes inside the IIFE, so the codes from other people won't affect our codes.

## **Closures - 闭包**

- An inner function has always access to the **variables** and **parameters** of its outer function, even after the outer function has returned.
    - we need to return a function, so inner function and outer function.
    - `score` and `value` can be access in the inner function, even though `score()`is already returned.

    变量 keepScore 指定了函数自我调用的返回字值。

    自我调用函数只执行一次。设置计数器为 0。并返回函数表达式。

    keepScore 变量可以作为一个函数使用。非常棒的部分是它可以访问函数上一层作用域的计数器。

    这个叫作 JavaScript 闭包。它使得函数拥有私有变量变成可能。

    计数器受匿名函数的作用域保护，只能通过 keepScore 方法修改。

    ```jsx
    function score() {
        var score = 0;
        
        return function(value) {
            score += value;
            return score;
        }
    }

    // score will store the return function and 
    // the value of variable a.
    var keepScore = score();
    ```

## Bind, Call and Apply

```jsx
var objA = {
	...
	methodName: function(parameterA, parameterB) {
		...
	}
};
```

**`call`**

`objA.methodName.call(**objB**, parameterA, parameterB);`

- method borrowing - objB is calling objA's method.
- **directly run**.

**`apply`**

`objA.methodName.apply(objB, [parameterA, parameterB]);`

- directly run.
- similar to `call`, but the parameters for the method is defined in an array.
- and, **the method should pass an array parameter.**

**`bind`**

`var functionName = objA.methodName.bind(objB, parameterA);`

- calling the function: `functionName(parameterB);`
- similar to `call`(can perform method borrowing as well), but bind will **return a function** and we need to store it somewhere.
- but, bind **doesn't call the function immediately,** we can **preset part/all the argument** by doing with `bind`.
- `bind` can allow to set **part of parameters** to the function and define the rest when we actually call it.
- **OR we can bind a Preset function by using** `bind`
    - `isFullAge.bind(this, 20)` still need to enter `el` argument, which will be added through `fn(arr[i])` → `isFullAge(arr[i])`
    - `20` is for `limit`
    - `this`, we don't need to worry about method borrowing here, because the object which call this method will be `bind` in this method.`**this**.isFullAge.bind(this, 20)`

    ```jsx
    var years = [1990, 1965, 1937, 2005, 1998];

    function arrayCalc(arr, fn) {
    	var arrRes = [];
    	for (var i = 0; i < arr.length; i++) {
    		arrRes.push(fn(arr[i]));
    	}
    	return arrRes;
    }
     
    function calculateAge(el) {
    	return 2020 - el;
    }

    function isFullAge(limit, el) {
    	return el >= limit;
    }

    var ages = arrayCalc(years, calculateAge);
    var fullAus = arrayCalc(ages, **isFullAge.bind(this, 20)**);
    ```

# Module

## Using Module

- we can return an object containing all of the functions that we want them to be public

### Create a Module pattern

`*budgetController`, `UIController` and `controller` are three different modules doing different tasks. `controller` is the main app, which passes other modules in.*

- `x` and `add()` cannot be accessed by public.
- `budgetController` and `UIController` they don't know each other, so we need a third module to conect them. → `controller`
- `function(budgetCtrl, UICtrl){}` this function is called after defining it - `(theFunction)(parameters)`
- `add(b)` will return the result of `23 + b`, and `publicTest(b)` will return this result

```jsx
var budgetController = (function() {
	var x = 23;

	var add = function(a) {
		return x + a;
	}

	return {
		publicTest: function(b) {
			return add(b); 
		}
	}
})();

var UIController = (function() {
    // some code
})();

var controller = (function(budgetCtrl, UICtrl) {

    var z = budgetCtrl.publicTest(5);
    
    return {
        init: function() {
					// calling other functions
        }
    }
})(budgetController, UIController);

// the InInitialisation Function
controller.init();
```

## Insert HTML from JS - AdjacentHTML

### Insert AdjacentHTML

- Syntax
    - `element.insertAdjacentHTML(position, text);`
        - `element` can be got by `getElementById` or `querySelector`
- Four positions
    - `beforebegin`
    - `afterbegin`
    - `beforeend`
    - `afterend`

    ```html
    <!-- beforebegin -->
    <p>
    	<!-- afterbegin -->
    	foo
    	<!-- beforeend -->
    </p>
    <!-- afterend -->

    ```