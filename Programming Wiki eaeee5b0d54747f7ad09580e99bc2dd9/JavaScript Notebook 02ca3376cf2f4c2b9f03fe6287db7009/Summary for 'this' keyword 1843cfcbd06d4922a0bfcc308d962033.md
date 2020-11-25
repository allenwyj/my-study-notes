# Summary for 'this' keyword

Created: Jun 29, 2020
Created by: Allen Wu
Tags: JavaScript

At the beginning of learning JavaScript, I often got confused and made mistakes while idetifying the object that 'this' points to. So, I have started to understand and summarise how to use 'this' keyword.

'this' depends on its execution context, which means the value of 'this' is determined as soon as the function which 'this' is inside gets called. It won't be able to be chaged during execution.

### General rule to follow:

How the function is **called** ( by ****who, when and from where), we don't need to care about how the function is declared or defined.

Example 1:

```jsx
function calculateAge(year) {
	console.log(this); 
}

calculateAge(1995); // print: window object
```

`calculateAge()` actually gets called by the `window` object.

Normally, when we declare and call a function, `this` inside the function will point to `window` object. However, under the `'use strict'` mode, `this` is undefined.

Example 2:

```jsx
const objA = {
	name: 'obj A',
	print: function() {
		console.log(this);
	}
};
objA.print(); // {name: 'obj A',print: function() {console.log(this);}}
```

`print()` is called by `objA`, so `this` points to the object which called it.

---

What if we have another function declared inside `print()`?

```jsx
const objA = {
	name: 'obj A',
	print: function() {
		console.log(this); // print: objA object

		**function innerFunction() {
			console.log(this); // print window object
		}
		innerFunction();**
	}
};
objA.print();
```

The first `this` will still point to `objA`, but the second `this` will point to `window` object. Why?

- Because `innerFunction()` is a still regular function declared and called even though it's inside an object. The object who called `innerFunction()` is `window`

How can we make `this` of `innerFunction()` points to `objA` object? We can `bind` the object which we want it to call our function:

- `var innerFunction = innerFunction.bind(objA);`

### 'this' in call-back function

`this` in **call-back** functions are similar to this `innerFunction`, it will print `window` object as well.

```jsx
function callCallBack(callback) {
	callback();
}
function printThis(){
	console.log(this);
}
callCallBack(printThis); // output: window object
```