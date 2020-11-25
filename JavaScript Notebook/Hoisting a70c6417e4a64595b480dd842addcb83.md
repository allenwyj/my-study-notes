# Hoisting

## Variable Hoisting

- `var` is **function-scoped**. Because `var`  allows the compiler to create the variable to `undefined` in the creation phase (**Hoisting**).

    ```jsx
    function test() {
    	console.log(a); // prints: undefined
    	var a = 1;
    	console.log(a); // pinrts: 1
    }

    test();

    // Behind the scene
    function test() {
    	var a;
    	console.log(a);
    	a = 1;
    	console.log(a);
    }
    test(); 
    ```

- Another Example
    - global variable is replaced by the local variable.

    ```jsx
    console.log(a);
    var a = 1;
    function test() {
      console.log(a);
      var a = 2;
      console.log(a);
    }
    test();
    console.log(a);

    // undefined
    // undefined
    // 2
    // 1
    ```

## Function Hoisting

- Functions also have hoisting.
- Function declaration and function expression will have different hoisting results.

### Function Declaration

```jsx
console.log(test1);
function test1() {
	console.log('I am test1');
}

// prints: *f test1 () { console.log(*'I am test1'); }
```

**The Actual Exectuion Order**

```jsx
function test1() {
	console.log('I am test1');
}
console.log(test1);
```

### Function Expression

```jsx
test1();
var test1;

function test1() {
	console.log('I am test1');
}

test1 = function() {
	console.log('I am another test1');
}

// result:
// I am test1
```

- The reason why it doesn't print `undefined` is because JavaScript will hoist function declaration first, then variable.