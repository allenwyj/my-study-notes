# JavaScript - ES6

# New Features

- Variable Declarations with `let` and `const`
- Blocks and IIFEs
- Strings
- Arrow Functions
- Destructuiring
- Arrays
- The Spread Operator
- Rest and Default Parameter
- Maps
- Classes and subclass.
- Promises
- Native Module

# Variable Declarations with `let` and `const`

- `const`
    - cannot change once it gets defined - like `final` in Java
    - `const` must be initialised when it is declared.
    - `const` is also block-scoped.
- `let`
    - Similar to `var`, but `let` is block-scoped, `var` is function-scoped.
    - If you declare and define a `var` variable inside a `if-else` statement, and you **can still access** them **outside** the `if-else` statement, since they are in the same **function**.
    - But, you cannot access the `let` variable like that. Because it is outside of the if-else block.
        - `let` cannot be affected by the new declared `let` variable with the same name which is from the outter or inner block. `const` is the same

            ```jsx
            function a() {
                let a = 3;
            		const b = 5;
                var c = 10;

                if (true) {
                    let a = 4;
            				const b = 10;
            				var c = 20;
                    console.log(a); // output: 4
            				console.log(b); // output: 10
            				console.log(c); // output: 20
                }
                
                console.log(a); // output: 3
            		console.log(b); // output: 5
            		console.log(c); // output: 20
            }

            a();
            ```

    - Also, `let` cannot be used before it was really declared. (unlike `var` is undefined)

# Blocks and IIFEs

## Block

- The blocks are not invoked. The block is not a function, it cannot be invoked and take arguments and return values.
- The code will be immediately executed line by line and no need to be called when the interpreter encounters this block.
- Simply create by using `{}`

    ```jsx
    {
    		const a = 1;
    		let b = 2;
    }

    console.log(a + b); // error: a is not defined.
    ```

## IIFEs - Increase Data Privacy

### ES5

```jsx
(function() {
		var c = 3;
})();

console.log(c); // c cannot be accessed due to old IIFE
```

### ES6

```jsx
{
	let a = 1;
	const b = 2;
	var c = 3;
}

console.log(a + b); // a and b can't be accessed due to IIFE
console.log(c); // prints 3; **because var is function-scoped**

c += 1;

console.log(c); // prints 4.
```

# Object

## Object Initialiser

- In ES6, objects can be created with **computed keys:** Object Initializer spec.
    - with [], you can do JS code for the object field name.

    ```jsx
    // Shorthand property names (ES2015)
    let a = 'foo', b = 42, c = {};
    let o = {a, b, c}

    // Shorthand method names (ES2015)
    let o = {
      property(parameters) {}
    }

    // Computed property names (ES2015)
    let prop = 'foo'
    let o = {
      [prop]: 'hey',
      ['b' + 'ar']: 'there'
    }
    ```

# Strings

- Using template Literal - ``...`` and using **template literal** `${}` to access variables inside the ```` (backtick)

    ```jsx
    let firstName = 'Allen';
    let lastName = 'Wu';
    const yearOfBirth = 1995;
    function calAge(year) {
    	...
    }
    // ES6
    console.log(`This is ${firstName} ${lastName}. Today he is ${calAge(yearOfBirth)} years old.`);
    ```

- Useful methods

    ```jsx
    const n = `${firstName} ${lastName}`; // 'Allen Wu'
    n.startsWith('A'); // output: true
    n.endsWith('Wu'); // output: true
    n.includes(' '); // output: true
    firstName.repeat(5); // AllenAllenAllenAllenAllen
    ```

- using eval()

    ```jsx
    const count = eval('1+1/2'); // '1+1/2' -> 1.5
    const count = eval('1-1/2'); // '1-1/2' -> 0.5
    ```

# Arrow Function

## What is Arrow Function

- It is a function that we can use to **replace** using `function` and `return` keyword in some cases.

    ```jsx
    // no argument
    let arrowFunc1 = () => console.log('No argument'); // no argument
    arrowFunc();

    // using one argument
    let arrowFunc2 = anArgument => console.log(anArgument);
    arrowFunc('Hello'); // output: Hello

    // multiple arguments and multiple lines statement
    let arrowFunc3 = (arg1, arg2) => {
    	console.log(arg1);
    	console.log(arg2);
    };
    arrowFunc3('Halo', 'Hi'); // output: Hello Hi
    ```

## Curly Brackets and Parentheses in Arrow Function

Arrow functions allow you to miss off the `return` keyword when the return value is a single expression:

```jsx
const add = (a, b) => a + b;
```

Just returning an object is that. However, curly brackets are also used by JS to denote the body of a function. So this:

```jsx
const testing = (x, y) => { foo: x, bar: y }
```

Is not gonna do what you think it’s gonna do (return an object). It will normally throw an error, because it thinks you’re doing:

```jsx
function testing (a, b) {
  return foo: a, bar: b;
}
```

So to fix this, wrap the value you want to return in parentheses, so it evaluates properly as an object:

```jsx
const testing = (x, y) => ({ foo: x, bar: y })

// Same asfunction testing (a, b) {
  return { foo: a, bar: b };
}
```

## Implicit Return

- So in the arrow function, if the statement is only one line, **and it isn't wrapped by curly brackets,** then it can apply an implicit return when it is necessary.
    - `const getInput = () => document.querySelector('.search_field').value;`
        - It will return a value.
- If the one line statement is wrapped by curly brackets, then it will not return anything.

## Using => for call-back Function

- Using for calling simple call-back function. using `=>`
    - before the arrow is the parameter need to pass to the codes after the arrow.
    - the code after the arrow will be the return statement.
- If we need to have more than one line statement after the arrow, then we need to use curly bracket and the `return` keyword is explict.
    - **no argument** or **more than one** argument: using `()`

```jsx
const years = [1990, 1995, 2000, 2005];

// ES5
var ages5 = years.map(function(currentValue) {
	return 2020 - currentValue;
});
console.log(ages5); // [30, 25, 20, 15]

// ES6
var ages6 = years.map(currentValue => 2020 - currentValue);
console.log(ages6); // [30, 25, 20, 15]

// two arguments, using ()
ages6 = years.map(**(**currentValue, index**)** => `Age element is ${index + 1}: ${2020 - currentValue}.`);

// more than one statement, using {} and return keyword
ages6 = years.map((currentValue, index) => **{**
	const now = new Date().getFullYear();
	const age = now - currentValue;
	**return** `Age element is ${index + 1}: ${age}.`;
**}**);
```

## Using => for 'this' Keyword

### Lexical this Variable.

- **arrow function do not have a `this` keyword. They simply get the `this` keyword of the function they are written in. Share the lexical `this` keyword from its surroundings. (有点在上一层找this的感觉）**
    - Avoid the problem when we are calling a callback function inside an Object (**The Problem**: this keyword points to window object)
- An arrow function does not have its own this. The this value of the enclosing lexical scope is used; arrow functions follow the normal variable loopup rules.
    - 箭头函数没有自己的this值
    - 箭头函数中使用的this都是来自函数的作用域链
    - 取值遵循普通变量一样的规则，在函数作用域链中一层一层往上找

### **Rules to follow to avoid 'this' problem**

- 对于需要使用object.method()方式调用的函数，使用普通函数定义，不要使用箭头函数。对象方法中所使用的this值有确定的含义，指的就是object本身。
- 其他情况下，全部使用箭头函数。

```jsx
// ES5
var box5 = {
	color: 'green',
	position: 1,
	clickMe: function() {
		**var self = this;**
		document.querySelector('.green').addEventListener('click', function() {
			var str = 'This is box number ' + self.position + ' and it is ' + self.color;
			alert(str);
		});
	}
}
box5.clickMe();

// ES6
// by using arrow function, 'this' keyword in the call-back function is
// now pointing to the box6 object instead of the window object
var box6 = {
	color: 'green',
	position: 1,
	clickMe: function() {
		document.querySelector('.green').addEventListener('click', () => {
			var str = 'This is box number ' + this.position + ' and it is ' + this.color;
			alert(str);
		});
	}
}
box6.clickMe();

---------------------------------------------------
function Person(name) {
	this.name = name;
}

// ES5
Person.prototype.myFriends5 = function(friends) {
		console.log(this); // points to Person object.
		var arr = friends.map(function(el) {
			console.log(this); **// points to window object.**
			return this.name + ' is friends with ' + el;
		});
		console.log(arr);
};

**// trick to avoid 'this' problem, or we can use var self = this.**
Person.prototype.myFriends5 = function(friends) {
		console.log(this); // points to Person object.
		var arr = friends.map(function(el) {
			console.log(this); /**/ points to Person object.**
			return this.name + ' is friends with ' + el;
		}**.bind(this)**);
		console.log(arr);
};

var friends = ['a', 'b'];
new Person('Allen').myFriends5(friends);

// ES6
Person.prototype.myFriends6 = function(friends) {
		console.log(this); // points to person object.
		var arr = friends.map(el => {
			console.log(this); // points to person object.
			return `${this.name} is friends with ${el}`; // this.name: Allen
		});
		console.log(arr);
};

```

---

# Destructuring

- Destructuring is a convenient way to extract data from a data structure like an object or an array. Storing each element to a single variable.

```jsx
// ES5
var allen = ['Allen', 25];
var name = allen[0];
var age = allen[1];

// ES6
**// destructuring an array**
const [name, age] = ['Allen', 25]; // name, age can be used now

**// destructuring an object**
const obj = {
	firstName: 'Allen',
	lastName: 'Wu'
};
// the variables should match the keys in obj.
const {firstName, lastName} = obj;

// example 2
const {firstName: a, lastName: b} = obj;

// example 3
function calAgeRetirement(year) {
	const age = new Date().getFullYear() - year;
	return [age, 65 - age];
}

const [myAge, retirement] = calAgeRetirement(1995);
```

## Destructuring an array

- `const [name, age] = array;` it will create `const name` and `const age` to store these data.
- Destructuring an array and append to another array.

```jsx
const a = [1, 2, 3, 4, 5, 6];
const b = [...a, 7, 8]; //[1, 2, 3, 4, 5, 6, 7, 8]
```

## Destructuring an object

- **variable names should exactly match the properties of the object.**

### Rename Property's Name

- if we want to use our own variable's names, do `firstName: a` (example 2).
- Practical case is to get data from a function which returns multiple data. (example 3)

### Basic Assignment

```jsx
const user = {
    id: 42,
    is_verified: true
};

const {id, is_verified} = user;

console.log(id); // 42
console.log(is_verified); // true
```

### Destructuring an object inside another object

```jsx
const state = {
	user: {
		currentUser: 'Allen'
	}
};

const { user: { currentUser } } = state;

console.log(currentUser); // prints: 'Allen'
```

### Destructuring Assignment - Rest in Object Destructuring

- Rest Parameters
    - The rest parameter collects parameters and turns them

    ```jsx
    // Rest properties collect the remaining own enumerable property keys that are not already picked off by the destructuring pattern.
    let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}
    a; // 10 
    b; // 20 
    rest; // { c: 30, d: 40 }

    const sections = [
    	{
        title: 'hats',
        imageUrl: 'https://i.ibb.co/cvpntL1/hats.png',
        id: 1,
        linkUrl: 'hats'
      }
    ];
    sections.map(({id, **...otherSectionFields**}) => (
    	console.log(otherSectionFields); // {title: "hats", imageUrl: "https://i.ibb.co/cvpntL1/hats.png", linkUrl: "hats"}
    ))
    ```

- Every other key value pair we want to pass in
    - In `({id, ...otherSectionFields})` - the rest in object is assigned to `otherSectionFields` variable.
    - `{...otherSectionProps}` is equivalent to `title={title} imageUrl={imageUrl} linkUrl={linkUrl}`

    ```jsx
    <div className="directory-menu">
      {
        // destructuring each section and passing them into MenuItem
        this.state.sections.map((**{ id, ...otherSectionProps }**) => (
          // desturcturing and pass the rest fields of section, and use the same name as the field name.
          <MenuItem key={id} **{...otherSectionProps}** />
        ))
      }
    </div>
    ```

# Spread Operator

1. When three dots (…) is at the end of function parameters, it's "rest parameters" and gathers the rest of the list of arguments into an array.
2. When three dots (…) occurs in a function call or alike, it's called a "spread operator" and expands an array into a list or an object into another object.
- `...` expands the `ages` array → `...ages`

```jsx
// use to pass an array as the parameters
var ages = [18, 30, 12, 21];
const max3 = addFourAges(...ages); 
// same as const max3 = addFourAges(18, 30, 12, 21);

// combine multiple arrays
const array1 = [1, 2, 3, 5];
const array2 = [6, 7, 8, 9];
const combinedArray = [...array1, 0, ...array2];
```

# Arrays

## from() - Convert List

- Convert a NodeList object to an Array
    - In ES6, using `Array.from(obj)` to convert the list.

```jsx
// ES5
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

// OR
var listArray = Array.prototype.slice.call(theList);
listArray.forEach(function(cur) {
	cur.style.backgroundColor = 'dodgerblue';
});

// ES6
var listArray = Array.from(theList);
theList.forEach(cur => cur.style.backgroundColor = 'dodgerblue');
```

## for(... of ...) loop - Using continue and break

- we cannot use `continue` and `break` for the `forEach()` and `map()` method.
- In ES6, it introduces `for of` loop to perform `forEach()` but with using `continue` and `break`.

```jsx
for (const cur of theArray) {
	if (cur.className === 'box blue') {
		continue;
	}
	cur.textContent = 'I changed to blue!';
}
```

## findIndex()

- The `findIndex()` method returns **the index of the first element** in the array **that satisfies** **the** **provided testing function**. Otherwise, it returns -1, indicating that no element passed the test.
- `findIndex()` will use each current element and pass it into the callback function.
    - when the callback function returns `true`, then getting the index of the current element.

```jsx
ages.findIndex(cur => cur >= 18);
```

## find()

- find the element which matches the callback function

```jsx
ages.find(cur => cur >= 18);
```

## includes()

- The `includes()` method determines whether an array includes a certain value among its entries, returning `true` or `false` as appropriate.

```jsx
const array1 = [1, 2, 3];
console.log(array1.includes(2)); // expected output: true

const pets = ['cat', 'dog', 'bat'];
console.log(pets.includes('cat')); // expected output: true
console.log(pets.includes('at')); // expected output: false
```

## reduce()

- The `arr.reduce(callback(accumulator, currentValue)[, initialValue])` method executes a **reducer** function (that you provide) on each element of the array, resulting in single output value. - loop the array and use each current element in the callback function.
    - `accumulator` - it accumulates the callback's return values. 之前的累计数
    - `currentValue` - The current element being processed in the array
    - `initialValue` - A value to use **as the first argument to the first call of the callback**. If no `initialValue` is supplied, the first element in the array will be used as the initial `accumulator` value and skipped as `currentValue`.

    ```jsx
    const arr = [1, 2, 3, 4];
    const sum = arr.reduce((accu, cur) => **(*return)*** accu + current, 0); // implicit return
    return sum;

    const sum = arr.reduce((accu, cur) => {
    	// do somtehing here ...
    	***return*** accu + current; // need to have explicit return
    }, 0);
    ```

# Function Parameters

## Rest Parameters

- receive multiple single values and transform to an array when we call a function with multiple parameters.

    ```jsx
    // ES6
    function isFullAge(...years) {
    	console.log(years);
    }

    isFullAge(1990, 1995, 2000); // transform these parameters to array

    // Complicated situation
    // Taking the rest parameters as an array
    function isFullAge(limit, ...years) {
    	console.log(limit); // 16
    	console.log(years); // [1990, 1995, 2000]
    }

    isFullAge(16, 1990, 1995, 2000);
    ```

- difference between spread operator and rest parameters is:
    - the spread operator: using in the function call.
    - the rest operator: using in the function declaration.

## Default Parameters

- we use one or more parameters to make a function to be preset.
- In ES6, Default function parameters allow named parameters to be initialized with default values if no value or undefined is passed.

```jsx
function multiply(a, b = 1) {
  return a * b;
}
```

```jsx
// ES5
function Person(name, age, country) {
	**// setting default value**
	country === undefined ? country = 'Australia' ; country = country; 

	this.name = name;
	this.age = age;
	this.country = country;
}

var allen = new Person('Allen', 25);

// ES6
// setting default value when we define it.
function Person(name, age, **country = 'Australia'**) {
	
	this.name = name;
	this.age = age;
	this.country = country;
}

```

# Maps

- new key-value pair data structure in ES6
- key: can be any kind of **primitive value**, unlike the object which is string only.
- `const question = new Map();` - create a new map
- `question.set(key, value);` - create a new key-value pair.
- `question.get(key);` - get the value by key
- `question.size;` - get the size of the map
- `question.delete(key);` - remove one key-value pair;
- `question.has(key);` - check if the map has the matched key-value pair. return true or false
- `question.forEach((value, key) ⇒ {...});` - Loop through the map
    - forEach() will return 3 parameters: value, key, map

```jsx
// create a map
const question = new Map();

// create key-value pairs
// **question.set(key, value);**
question.set('question', 'What is the official name of the JS');
question.set(1, 'ES5');
question.set(2, 'ES6');
question.set(3, 'ES2015');
question.set(4, 'ES7');
question.set('correct', 3);
question.set(true, 'Correct answer');
question.set(false, 'Wrong answer');

// get the value by using key
// **question.get(key);**
question.get('question');

if (question.has('question') {
	question.delete('question');
}

// using for of loop
for (let key of question) {
	...
}

// using for of loop through the values
// using destructuring to store key and value
for (let [key, value] of **question.entries()**) { // return all entries in the map
	if (typeof(key) === 'number') {
		console.log(`Answer ${key}: ${value}`);
	}
}

const ans = parseInt(prompt('User Input')); // convert user input to number
```

# Classes

## Using class, constructor to create class

```jsx
// ES5
var Person5 = function(name, yearOfBirth, job) {
	this.name = name;
	this.yearOfBirth = yearOfBirth;
	this.job = job;
}

Person5.prototype.calculateAge = function() {
	var age = new Date().getFullYear - this.yearOfBirth;
	console.log(age);
}

var allen5 = new Person5('Allen', 1995, 'student');

**// ES 6**
class Person6 {
	// **MUST** have a constructor
	constructor (name, yearOfBirth, job) {
		this.name = name;
		this.yearOfBirth = yearOfBirth;
		this.job = job;
	}
	
	// adding prototype method
	calculateAge() {
		var age = new Date().getFullYear - this.yearOfBirth;
		console.log(age);
	}
}

const allen6 = new Person6('Allen', 1995, 'student');
```

## Field Declarations

### Public Field Declarations

- the fields can be declared with or without a default value.

```jsx
class Rectangle {
  height = 0;
  width;
  constructor(height, width) {    
    this.height = height;
    this.width = width;
  }
}
```

### Private Field Declarations

- Private fields can only be declared up-front in a field declaration.
- Private fields cannot be created later through assigning to them, the way that normal properties can.

```jsx
class Rectangle {
  #height = 0;
  #width;
  constructor(height, width) {    
    this.#height = height;
    this.#width = width;
  }
}
```

## Static Method

- Static method is not inherited by the class instances, it is attached to the instance.
- a class-level method

```jsx
// ES 6
class Person6 {
	// **MUST** have a constructor
	constructor (name, yearOfBirth, job) {
		this.name = name;
		this.yearOfBirth = yearOfBirth;
		this.job = job;
	}
	
	// adding prototype method
	calculateAge() {
		var age = new Date().getFullYear - this.yearOfBirth;
		console.log(age);
	}

	static greeting() {
		console.log('Hello');
	}
}

Person6.greeting();
```

## Sub-classes

### Inheritance

- For ES5

```jsx
// Creating the subclass of Person Class
// ES5
var Person5 = function(name, yearOfBirth, job) {
	this.name = name;
	this.yearOfBirth = yearOfBirth;
	this.job = job;
}

Person5.prototype.calculateAge = function() {
	var age = new Date().getFullYear() - this.yearOfBirth;
	console.log(age);
}

var Athlete5 = function(name, yearOfBirth, job, games, medals) {

	// Using Person5 constructor to new Athlete5 object
	Person5.call(**this**, name, yearOfBirth, job); // passing the parameters
	this.games = games;
	this.medals = medals;
	
}

// getting the Person5 prototype as the prototype of Athlete5
Athlete5.prototype = Object.create(Person5.prototype);

// creating the sub-class's own method
Athlete5.prototype.wonMedals = function() {
	this.medals++;
	console.log(this.medals);
}

// create the new instance
var allenAthlete5 = new Athlete5('Allen', 1995, 'runner', 3, 10);

allenAthlete5.wonMedals();
allenAthlete5.calculateAge();
```

---

- **For ES6 - Preferred way**

```jsx
// ES6
**class** Person6 {
	// **MUST** have a constructor
	**constructor** (name, yearOfBirth, job) {
		this.name = name;
		this.yearOfBirth = yearOfBirth;
		this.job = job;
	}
	
	// adding prototype method
	calculateAge() {
		var age = new Date().getFullYear() - this.yearOfBirth;
		console.log(age);
	}
}

**// sub-class**
class Athlete6 **extends** Person6 {
	constructor(name, yearOfBirth, job, games, medals) {
		**super(name, yearOfBirth, job);** // call the super class's constructor
		this.games = games;
		this.medals = medals;
	}

	// sub-class's own method
	wonMedal() {
		this.medals++;
		console.log(this.medals);
	}
}

const allenAthlete6 = new Athlete6('Allen', 1995, 'runner', 5, 10);
allenAthlete6.wonMedals();
allenAthlete6.calculateAge();
```

# Asynchronous

- Allow asynchronous functions to run in the 'background'
- We pass in callbacks that run once the function has finished its work
- Move on immediately: Non-blocking

## Web APIs

- live outside of the JavaScript engine.
- **Event loop**
    - our **event** sits in the **Web APIs** environment and waiting for a certain event to happen, like the timer.
    - when the **event** happens, then the **callback function** is placed to the **Message Queue** and wait to be fired as soon as the execution context is empty.
    - the **event loop** will **push** the **first callback function** in line (may have many callback functions are waiting in the message queue) onto the Execution Stack as soon as the execution context is empty.

### setTimeOut()

- `setTimeOut((parameter) ⇒ {}, waitTime, parameterToSetTimeOut);`
    - `parameter` - the argument that is passed to the `setTimeOut()`
    - `waitTime` - the waiting time in milliseconds to fire the callback function.
    - `parameterToSetTimeOut` - the parameter that we want to pass to the callback function.

```jsx
const second = () => {
	setTimeout(() => {
	    console.log('Async Hey there');
	}, 2000);
	
	console.log('Second');
}
	
const first = () => {
	console.log('Hey there');
	second();
	console.log('The end');
}
	
first();
```

## The traditional way for Asynchronous

### callback hell

- one Asynchronous method inside another one, again and again.

```jsx
function getRecipe() {
    setTimeout(() => {
        const recipeID = [523, 883, 432, 974];
        console.log(recipeID);

        setTimeout(id => {
            const recipe = {title: 'Fresh tomato paste', publisher: 'Allen'};
            console.log(`${id}: ${recipe.title}`);

            setTimeout(publisher => {
                const recipe2 = {title: 'pizza', publisher: 'Allen'};
                console.log(recipe2);
            }, 1500, recipe.publisher);

        }, 1000, recipeID[2]);

    }, 1500);
}

getRecipe();
```

## The modern way for Asynchronous

### Promise

- Promise is an object that keeps track about whether a certain asynchronous event has happened already or not;
- it determines what happens after the event has happened;
- it implements the concept off a future value that we're expecting.

**Pending → (event happens) → settled / resolved → fulfilled or rejected**

pending - when the event is waiting

settled / resolved - when the event happens

fulfilled - when the event returns a result

rejected - when the event return an error.

So, we will create a promise function and doing some tasks in the callback function. If the event executes successfully, we will `then` consume the returned result of the promise

- `resolve` - if successful, taking the result as the parameter of `resolve`
- `reject` - if unsuccessful,
- `then()` - takes the returned result and consumes the promise when the promise is **successful**. (fulfilled promise)
- `catch()` - takes the error and consumes the rejected promise (unsuccessful promise)

```jsx
**// getIDs is a promise, so it is waiting to be
// resolved, getRecipe and getRelated are functions.**
const getIDs = new Promise((resolve, reject) => {
  setTimeout(() => {
    // saying successful, and return result.
    resolve([523, 883, 432, 974]); 
  }, 1500);
});

const getRecipe = recID => {
	return new Promise((resolve, reject) => {
		setTimeout(ID => {
			const recipe = {title: 'Fresh tomato paste', publisher: 'Allen'};
	    resolve(`${ID}: ${recipe.title}`);
		}, 1500, recID);
		
	});
};

const getRelated = publisher => {
  return new Promise((resolve, reject) => {
    setTimeout(pub => {
      const recipe = {title: 'Pizza', publisher: 'Allen'};
      resolve(`${pub}: ${recipe.title}`);
    }, 1500, publisher)
  });
};

// .then() consumes the promoise when the event executes successfully
// getting the IDs from the first promise and use it for
// the second promise
**// promise return chain**
getIDs
.then(IDs => {
  console.log(IDs); // IDs: [523, 883, 432, 974]
	**return getRecipe(IDs[2]); // return a promise** 
})
**.then(receipe => { // handle the next promise result
	console.log(receipe); // 432: Fresh tomato paste
	return getRelated('Wu');
})
.then(pub => {
  console.log(pub); // Wu: Pizza
})**
.catch(error => {
	console.log('Error!');
});
```

### Using Async and Await

- **Instead** of using **promise return chain**, we can use async/await

`async` - saying this function is an Asynchronous function

- The `async` function **returns a promise**
    - **It allows `async` function to return something by using `.then()`**
- We can have one or more `await` expressions.

`await` - waiting a promise to return a result

`const IDs = await firstPromise;` - `await` will stop the code from executing at this point, and wait until the `firstPromise` is fullfilled.

- then it will assign the result of the **resolved** **promise** to the `IDs` variable.
- if the `firstPromise` is **rejected**, then an error will be thrown.
- `await` **MUST** be inside of the `async` function
- using `try...catch` to handle the error.
- `async` function **always return a promise. If we are gonna consume the return value from a promise, we need to use `await` inside another `async` function or we need to use `.then` to resolve that promise. If we don't need the async return anything, we can simply call this function without using `await` or `.then`**

    Async functions always return a promise. If the return value of an async function is not explicitly a promise, it will be implicitly wrapped in a promise.

    For example, the following:

    ```jsx
    // Async arrow functions look like this:
    const foo = async () => {
      // do something
    }

    // Async arrow functions look like this for a single argument passed to it:
    const foo = async evt => {
      // do something with evt
    }

    // Async arrow functions look like this for multiple arguments passed to it:
    const foo = async (evt, callback) => {
      // do something with evt
      // return response with callback
    }

    // The **anonymous** form works as well:
    const foo = async function() {
      // do something
    }

    // An async **function declaration** looks like this:
    async function foo() {
      // do something
    }

    // Using async function in a **callback**:
    const foo = event.onCall(async () => {
      // do something
    })

    ```

    ...is equivalent to:

    ```jsx
    function foo() {
       return Promise.resolve(1)
    }
    ```

```jsx
async function getRecipesAW() {
    const IDs = await getIDs;
    console.log(IDs);
		const recipe = await getRecipe(IDs[2]); // getRecipe() returns a promise
    console.log(recipe);
    const related = await getRelated('Allen');
    console.log(related);

		return recipe;
}

// async function returns a promise
getRecipesAW().then(result => console.log(result));
```

### In-depth Understanding Promise

- 对于 JS 来说，代码执行永远是同步的，异步是依靠浏览器来做的。
- 对于浏览器来说，在处理异步的时候有好几个队列，**promise 是放在一个 Microtask** 队列中的。其他几个虽然也存储在不同的队列，但是本质是相同的。
- 浏览器有一个 Event Loop 在循环检查执行栈是否为空，为空时就丢异步代码进去。
- **Microtask** 队列中的任务会在每次 Event Loop 循环**结束之前**执行。而其他队列中的任务会在每次开始**新的 Event Loop 时**执行。所以 promise 会先于其他几个异步任务执行。
- Promise 是异步的，指的是它的`then()`和`catch()`方法，Promise的本身内部函数还是同步执行的。

    ```jsx
    let a = new Promise(
      (resolve, reject) => {
        console.log(1)
        setTimeout(() => console.log(2), 0)
        console.log(3)
        console.log(4)
        resolve(true)
      }
    )
    a.then(v => {
      console.log(8)
    })
    // prints: 1 3 4 8 2
    ```

    [JavaScript同步、异步、回调执行顺序分析_Wendy-CSDN博客](https://blog.csdn.net/u010297791/article/details/71158212)

- **Promise 链式调用顺序思考**

    ```jsx
    new Promise((resolve, reject) => {
      console.log("log: 外部promise");
      resolve();
    })
      .then(() => {
        console.log("log: 外部第一个then");
        new Promise((resolve, reject) => {
          console.log("log: 内部promise");
          resolve();
        })
          .then(() => {
            console.log("log: 内部第一个then");
          })
          .then(() => {
            console.log("log: 内部第二个then");
          });
      })
      .then(() => {
        console.log("log: 外部第二个then");
      });

    // log: 外部promise
    // log: 外部第一个then
    // log: 内部promise
    // log: 内部第一个then
    // log: 外部第二个then
    // log: 内部第二个then
    ```

    [Promise 链式调用顺序引发的思考](https://juejin.im/post/6844903972008886279)

# AJAX

**Asynchronous JavaScript And XML**

- communicate with the reomote server and run it in the background (**asynchronous**)
- like using API

跨域问题 JSONP CORS

[Axios or fetch(): Which should you use? - LogRocket Blog](https://blog.logrocket.com/axios-or-fetch-api/)

## fetch()

- `fetch()` will return a promise which we need to handle it
- `.json()` will return a promise either.
- `fetch()` uses `body` property to **send data**.
- The `data` in body is **stringified.**
- `URL` is passed as the **first** argument into `fetch()`.

```jsx
// fetch()

const url = 'http://localhost/test.htm';
const options = {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json;charset=UTF-8'
  },
  body: JSON.stringify({
    a: 10,
    b: 20
  })
};

fetch(url, options)
  .then(response => {
    console.log(response.status);
  });
```

```tsx
// fetch()

const url = 'http://localhost/test.htm';
const options = {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json;charset=UTF-8'
  },
  **body: JSON.stringify({
    a: 10,
    b: 20
  })**
};

fetch(url, options)
  .then(response => {
    console.log(response.status);
  });
```

```jsx
// fetch() using async/await

fetch('https://cors-anywhere.herokuapp.com/https://www.metaweather.com/api/location/2487956/')
	.then(result => {
    console.log(result);
    return result.json(); // .json() is a  new promose
	})
	.then(data => {
    console.log(data);
	})
	.catch(error => {
    console.log('Error!');
	});

// Using async and await
async function getWeatherAW(woeid) {
	try {
    const result = await fetch(`https://cors-anywhere.herokuapp.com/https://www.metaweather.com/api/location/${woeid}/`);
    const data = await result.json();
    console.log(data);
		return data;

	} catch(error) {
		console.log(error);
	}
}

getWeatherAW(2487956); // San Fancisco

let dataLondon = getWeatherAW(44418) // London
	.then(data => {
		dataLondon = data;
		console.log(dataLondon);
	});
```

## axios - in order to perform fetch() for AJAX

***Promise based HTTP client for the browser and node.js***

- For now (June-2020), fetch() is still not fully implemented by all browsers. So we need to use axios to perform fetch task.
- **Install in the project folder**
    - `npm install axios --save`
    - It's not a dev tool.
- **Import it from the dependency** when we need to use it. - we don't need to specify the path because it will search in the dependencies.
    - `import axios from 'axios';`
- axios is a way more intuitive and better when it is handling the error
- Axios uses `data` property to **send data**.
- `data` property will be **converted** into JSON by Axios automatically.
- `URL` is set in the **object** that are passed into `axios`.

### GET Request

```tsx
import axios from 'axios';

**async** function getResult(query) {
  try {
    const res = **await** **axios**(`https://forkify-api.herokuapp.com/api/search?&q=${query}`);
    const recipes = res.data.recipes;
    console.log(recipes);
  } catch (error) {
    alert(error);
  }
}
getResult('pizza');
```

### POST Request

- Axios automatically converts the `data` property into JSON format.
- Send a POST request with custom headers to a URL.

    ```tsx
    const options = {
      url: 'http://localhost/test.htm',
      method: 'POST',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json;charset=UTF-8'
      },
      **data: {
        a: 10,
        b: 20
      }**
    };

    axios(options)
      .then(response => {
        console.log(response.status);
      });
    ```

```jsx
// The other method.
const onToken = token => {
  axios({
    url: 'payment',
    method: 'post',
    data: {
      amount: priceForStripe,
      token
    }
  })
  .then(response => {
    alert('Payment Successful');
    dispatch(clearCart());
  })
  .catch(error => {
    console.log('Payment error: ', JSON.parse(error));
    alert(
      'There was an issue with your payment. Please make sure you use the provided credit card.'
    );
  });
};
```

## fetch() or Axios

# Persistent Data - localStorage

## Using localStorageAPI

- `localStorage` is a function that lives in on the window object (the global object). It's available to us everywhere.
- Storing into localStorage
    - `localStorage.setItem(key, value)`
        - `key`: MUST be string
        - `value`: MUST be string
            - In order to convert a JSON object to a String object, we can use `JSON.stringify(myObject)`
            - convert string back to JSON object, we can use `JSON.parse(myObject)`
- Getting data from the localStorage
    - `localStorage.getItem(key)`
- Removing data from the localStorage
    - `localStorage.removeItem(key)`
- Getting the number of key-value pairs in the localStorage
    - `localStorage.length` ⇒ return number

# Installing Node.js and NPM

- download node.js and type `npm` inside the project folder to create **package.json** file
    - go to the project folder through the command line and type `npm init`
    - after the following setups, we can run `npm` command in this project folder.

## Setup the development dependency

- `--save-dev` and `--save` are installed locally in this project.
- `--global`install globally,
    - for example `npm install live-server --global`
- `npm install webpack --save-dev` to install the webpack.`--save-dev` means that it will save webpack as a development dependency of the project.

## Install the jquery

- `npm install jquery --save` to install the jquery. `--save` means that it will save jQuery as a dependency of the project.

## How to install all dependencies

- we can automatically install all dependencies listed in the package.json file. So we don't need to copy and paste the entire folder to another computer.
- `npm install` - this command checks the dependency files in the package.json and install them.
- `npm uninstall jquery` - delete the dependency.

## webpack config - bundle multiple js files

**File**: **webpack.config.js**

- `entry` - the entry property of this project, where the webpack will start the bundling.
    - we can specify more than one entry file.
- `output` - where to save the output bundle file.
    - `path` - MUST be an absolute path
        - `__dirname` - the current absolute path of the project
        - `'dist'` - the folder that we want our bundle.js in.
    - `filename` - a standard name for webpack output.
- `development mode` - simply builds our bundler without minifying our code, in order to be as fast as possible.
- `production mode` - will automatically enable all kinds of optimisation in order to reduce to final bundle size.

```jsx
File: webpack-config.js
// using build-in node package
const path = require('path'); 

module.exports = {
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'js/bundle.js'
    }
		//mode: 'development' // can move this to npm script
};

File: package.json
{
  "name": "forkify",
  "version": "1.0.0",
  "description": "forkify project",
  "main": "index.js",
  "scripts": { // npm script
    **"dev": "webpack --mode development", // npm run dev => set to development mode
    "build": "webpack --mode production" // npm run build => set to production mode**
  },
  "author": "Yujie Wu",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "jquery": "^3.5.1"
  }
}
```

- By using bundle, now we can use the export something via `export` keyword and then we can have a `default` export or a `named` export.
    - `export default 23;` - in test.js file.
    - then we can simply by using `import` from another js file and we don't need to specify the .js for the export file.
        - `import num from './test';` - in index.js
- add an npm script, in **package.json** file
    - changed `"test"` to `"dev"` which stands for development
    - and the value to `"webpack --mode development"` (to call webpack) - so as soon as we start the npm script called dev, it will open the webpack and loop up our configuration and do the work that we specify in the config file.
        - npm script is the best way for now to lanuch our locally installed dev dependency, like webpack.
- `npm install webpack-cli --save-dev` - install the webpack command line interface (something that allows us to access the webpack throught the command line interface.)
- To actually run our npm script in the command line, type `npm run dev`
    - it bundles our test.js and index.js together and output the bundle.js file.

## webpack server - stimulate a real http server

- Automatically bundle all our JavaScript files, and then reload the app in a browser whenever we change a file.
- When we are running the development server, **webpack will bundle our modules together, but it will actually not write it to a file on disk. It will automatically inject it into the html.**
- Install - run in command line
    - `npm install webpack-dev-server --save-dev`
- A script start will always be a script that keeps running in the background and updates the browser as soon as we changed the code. Add into package.json and `"script"` property.
    - `**"start": "webpack-dev-server --mode development --open"**`
        - `--open` - open up the page in the browser automatically
- Run the html page in the webpack server
    - stimulate a real http server
    - `npm run start`
- **specify the output folder is 'dist' which MUST be same as the contentBase folder**

```jsx
File: webpack.config.js

const path = require('path');

module.exports = {
    entry: './src/js/index.js',
    output: {
        path: path.resolve(__dirname, **'dist'**), // the output folder 'dist'
        filename: **'js/bundle.js'**
    },
    devServer: {
        contentBase: './dist' // specify from which webpack should serve our files
    }   
};
```

## webpack plugin - copy and use the template

- Install the webpack plugin
    - `npm install html-webpack-plugin --save-dev`
- require it in the webpack.config.js
- we use the index.html in src folder as the template and inject it into the index.html in dist folder.

    ```jsx
    const path = require('path');
    **const HtmlWebpackPlugin = require('html-webpack-plugin');**

    module.exports = {
        entry: './src/js/index.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'js/bundle.js'
        },
        devServer: {
            contentBase: './dist'
        }**,
        plugins: [
            new HtmlWebpackPlugin({
                filename: 'index.html',
                template: './src/index.html'
            })
        ]**
    };
    ```

## babel - convert codes to ES5

- `npm install --save-dev @babel/core @babel/preset-env babel-loader`
    - babel/core
        - contains the core functionality of the compiler
    - babel/preset-env
        - it will take care that all the modern JavaScript features are converted back to ES5 and process them. Like converting SASS to CSS Code or convert ES6 code to ES5 JavaScript.
    - babel-loader
        - loaders in webpack allow us to import or to load all kinds of different files
        - `test` - using regex to test these .js file at the end and if they are js file, then it will apply the babel loader
        - `exclude` - using regex, we dont need to include those package files.

        ```jsx
        **File: webpack.config.js**

        const path = require('path');
        const HtmlWebpackPlugin = require('html-webpack-plugin');

        module.exports = {
            ...
            **module: {
                rules: [
                    {
                        test: /\.js$/, // look for all files ending with .js
        								exclude: /node_modules/, // no need to include these files
                        use: {
                            loader: 'babel-loader'
                        }
                    }
                ]
            }**
        };
        ```

    - create babel config file
        - under the project folder, create a new file named **.babelrc** (standard name)
            - `presets` - a collectioin of code transform plugins which actually apply the transformation of our codes
                - `env` - standards for the environment

        ```jsx
        File: .babelrc
        {
            "presets": [
                ["@babel/env", {
                    "useBuiltIns": "usage",
                    "corejs": "3",
                    "targets": {
                        "browsers": [ **// which browsers we want to target**
                            "last 5 versions",
                            "ie >= 8" **// the ie is more than 8**
                        ]
                    }
                }]
            ]
        }
        ```

- `npm install --save core-js@3 regenerator-runtime`

### polyfill - for codes that are not in ES5

- convert those codes which are not in ES5, like promise, array.from
- `npm install babel-polyfill --save` (**we don't need to install babel-polyfill anymore**)
    - polyfill will be the code which is in our final code. It is not a development tool but it will be the code goes into our final bundle. So this is not a devdependency

## Install a third-part package

- using npm to install

### Fractional - 分数计算

- `npm install fractional --save`
- Fractional provides a simple interface to add, subtract, multiply, and divide fractions.
- Fractions are represented in most-normalized form.

```jsx
import { Fractional } from 'fractional';

var Fraction = require('fractional').Fraction

// Create a new fraction with the new keyword:

(new Fraction(7,3)).multiply(new Fraction(1,2)).toString()
// **7/3 * 1/2 = 1 1/6**

(new Fraction(7,3)).divide(new Fraction(1,2)).toString()
// **7/3 / 1/2 = 4 2/3**

(new Fraction(3,10)).add(new Fraction(5,9)).toString()
// 77/90

(new Fraction(0.25)).add(new Fraction(1,6)).toString()
// 5/12

(new Fraction(0.35)).subtract(new Fraction(1,4)).toString()
// 1/10

(new Fraction(5, 10)).numerator
// 5

(new Fraction(3, 4)).denominator
// 4
```

# Module in ES6

![JavaScript%20-%20ES6%202c0b7b255b8c46d2bfd5afb5813c264d/Untitled.png](JavaScript%20-%20ES6%202c0b7b255b8c46d2bfd5afb5813c264d/Untitled.png)

- Model里面的js文件首字母通常是大写

`export`

- `default` - export one per module
    - `export default 'I am an exported string.';`
    - `export default function functionName() {...}`
    - `export default class className {...}`
- `named` - export mutiple things from the same module
    - `export const add = (a, b) ⇒ a + b;`
    - `export const multiply = (a, b) => a * b;`
    - `export const ID = 23;`

`import` - we don't need to say the file type.

- simple import
    - `import str from './models/Search'`
- import multiple things - using **curly brackets** with the export names.
    - `import { add, multiple, ID } from './views/searchView';`
    - OR `import { add as a, multiple as b, ID as c } from './views/searchView';` - asign the different name for the imports.
    - OR `import * as searhView from './views/searchView';`

## Application State

***All things in one given moment are the state. All these data in the current state in a current moment of our app, like the current object, shopping list object, liked recipes object etc.***